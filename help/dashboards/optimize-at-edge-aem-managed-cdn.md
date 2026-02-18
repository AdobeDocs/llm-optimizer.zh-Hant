---
title: 在Edge最佳化 — AEM Cloud Service Managed CDN (Fastly)
description: 瞭解如何在LLM Optimizer中設定AEM Cloud Service Managed CDN (Fastly)以便在Edge最佳化。
feature: Opportunities
source-git-commit: 8cdd15413555057165f69ea4d5a15b243ab9098d
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 16%

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

{{verify-setup-adobe-aem-cs-cdn}}

{{return-to-overview}}
