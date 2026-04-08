---
title: 邊緣最佳化：AEM Cloud Service 管理的內容傳遞網路 (Fastly)
description: 了解在 LLM Optimizer 中如何設定 AEM Cloud Service 管理的內容傳遞網路 (Fastly) 進行邊緣最佳化。
feature: Opportunities
source-git-commit: 0c7ccadbb40c8c119cb2a57cf8118708c33c4236
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 76%

---


# AEM Cloud Service 管理的內容傳遞網路 (Fastly)

此設定會將代理式流量 (來自 AI 機器人和 LLM 使用者代理的要求) 路由至 Edge Optimize 後端服務 (`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中尋找 `x-edgeoptimize-request-id` 標頭。

**先決條件**

若要開始將代理流量路由至 Edge Optimize：

1. 在LLM Optimizer中，開啟&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

   ![導覽至客戶設定](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找到&#x200B;**將最佳化部署到AI代理程式**&#x200B;區段。 勾選&#x200B;**啟用最佳化引擎**&#x200B;核取方塊。

   ![將最佳化部署到AI代理程式 — 擱置中](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在確認對話方塊中，選取&#x200B;**啟用**。 Adobe 團隊將代表您處理路由設定。

   ![啟用最佳化引擎確認對話方塊](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

   設定好路由並啟用後，狀態會更新為&#x200B;**已完成的**，並加上綠色核取記號以確認路由已啟用。 您無需再執行任何動作。

   ![將最佳化部署到AI代理程式 — 已完成](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

此外，如果您對於上述步驟需要任何協助，請聯絡您的 Adobe 帳戶團隊或 `llmo-at-edge@adobe.com`。

**經由 Cloud Manager 管道的自助路由**

如果您偏好透過 Cloud Manager 管道自行設定路由，請按照下列步驟操作。 路由設定是使用 [originSelector 內容傳遞網路規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)所完成。 其先決條件如下所示：

* 決定要路由的網域。
* 決定要路由的路徑。
* 決定要路由的使用者代理 (建議的規則運算式)。

為了部署規則，您必須：

* 建立[設定管道](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/operations/config-pipeline)。
* 認可存放庫中的 `cdn.yaml` 設定檔案。
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

**驗證設定**

完成設定後，請確認機器人流量會路由至 Edge Optimize，而真人流量不受影響。

**1. 測試機器人流量 (應經過最佳化)**

運用代理式使用者代理模擬 AI 機器人要求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的回應會包含 `x-edgeoptimize-request-id` 標頭，確認要求已經透過 Edge Optimize 進行路由：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 測試真人流量 (不應受到影響)**

模擬一般真人瀏覽器要求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

回應&#x200B;**不應**&#x200B;包含 `x-edgeoptimize-request-id` 標頭。 頁面內容和回應時間應與啟用邊緣最佳化之前維持相同。

**3. 如何區分這兩種情境**

| 頁首 | 機器人流量 (最佳化) | 真人流量 (不受影響) |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在：包含唯一的要求 ID | 不存在 |
| `x-edgeoptimize-fo` | 唯有發生容錯移轉時存在 (值：`1`) | 不存在 |

**4. 檢查LLM Optimizer**&#x200B;中的路由狀態

您也可以在LLM Optimizer UI中確認路由。 開啟&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。 路由作用中時，**部署最佳化至AI代理程式**&#x200B;區段會顯示&#x200B;**已完成**。

![將最佳化部署到AI代理程式 — 已完成](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

{{return-to-overview}}
