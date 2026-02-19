---
title: 在Edge最佳化 — AEM Cloud Service Managed CDN (Fastly)
description: 瞭解如何在LLM Optimizer中設定AEM Cloud Service Managed CDN (Fastly)以便在Edge最佳化。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 12%

---


# AEM Cloud Service Managed CDN (Fastly)

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

若要開始將代理流量路由到Edge最佳化：

1. 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

   ![瀏覽至客戶組態](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**部署最佳化的AI流量路由**&#x200B;下，勾選&#x200B;**部署最佳化至AI代理程式**&#x200B;核取方塊。 Adobe團隊將代表您處理路由設定。

   ![勾選部署最佳化至AI代理程式](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 啟用核取方塊後，狀態會顯示設定進行中。 Adobe團隊將為您完成路由設定。

   ![AI流量路由設定進行中](/help/assets/optimize-at-edge/prereq-traffic-routing-progress.png)

   設定好製程並啟用後，狀態會更新為顯示綠色核取記號，表示已成功啟用製程。 您無需執行進一步的動作。

此外，如果您需要上述步驟的任何協助，請聯絡您的Adobe帳戶團隊或`llmo-at-edge@adobe.com`。

**透過Cloud Manager Pipeline的自助路由**

如果您偏好透過Cloud Manager Pipeline自行設定繞線，請遵循下列步驟。 路由設定是使用 [originSelector 內容傳遞網路規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)所完成。 其先決條件如下所示：

* 決定要路由的網域。
* 決定要繞線的路徑。
* 決定要路由的使用者代理程式（建議的規則運算式）。

為了部署規則，您必須：

* 建立[設定管道](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/operations/config-pipeline)。
* 認可存放庫中的`cdn.yaml`設定檔。
* 執行設定管道。

```
kind: "CDN"
version: "1"
data:
  # Origin selectors to route to Edge Optimize backend
  originSelectors:
    rules:
      - name: route-to-edge-optimize-backend
        when:
          allOf:
            - reqHeader: x-edgeoptimize-request
              exists: false # avoid loops when requests comes from Edge Optimize
            - reqHeader: user-agent
              matches: "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)" # routed user agents
            - reqProperty: domain
              equals: "example.com" # routed domain
            - reqProperty: originalPath
              matches: '(/[^./]+|\.html|/)$' # routed extensions, with .html extension or without extension
            - anyOf:
              - { reqProperty: originalPath, in: [ "/page.html" ] } # routed pages, exact path matching
              - { reqProperty: originalPath, like: "/dir/*" } # routed pages, wildcard path matching
        action:
          type: selectOrigin
          originName: edge-optimize-backend
    origins:
      - name: edge-optimize-backend
        domain: "live.edgeoptimize.net"
```

**驗證安裝程式**

完成設定後，請確認正在將機器人流量路由至Edge Optimize，且人力流量不受影響。

**1. 測試機器人流量（應該最佳化）**

使用代理使用者代理程式模擬AI機器人請求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的回應包含`x-edgeoptimize-request-id`標頭，確認要求已透過Edge最佳化路由：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 測試人力流量（不應受影響）**

模擬一般的人類瀏覽器請求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

回應應&#x200B;**不**&#x200B;包含`x-edgeoptimize-request-id`標頭。 在Edge中啟用「最佳化」之前，頁面內容和回應時間應保持相同。

**3. 如何區分這兩個案例**

| 頁首 | 機器人流量（最佳化） | 人類流量（未受影響） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在 — 包含唯一的請求識別碼 | 不存在 |
| `x-edgeoptimize-fo` | 只有在發生容錯移轉時才會出現（值： `1`） | 不存在 |

流量路由的狀態也可以在LLM Optimizer UI中檢視。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

{{return-to-overview}}
