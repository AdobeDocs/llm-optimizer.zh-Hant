---
title: 在Edge最佳化 — Akamai (BYOCDN)
description: 瞭解如何在LLM Optimizer中為Edge最佳化設定Akamai BYOCDN。
feature: Opportunities
source-git-commit: 23752b30294c3d467ca477b085aa521cad0f72ca
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 26%

---


# Akamai (BYOCDN)

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

在設定Akamai屬性管理員規則之前，請確定您已：

* 存取您網域的Akamai屬性管理員。
* 已完成LLM Optimizer上線程式。
* 已完成CDN記錄檔轉送至LLM Optimizer。
* 從Edge UI擷取的LLM Optimizer最佳化API金鑰。

{{retrieve-byocdn-api-key}}

**組態**

以下Akamai屬性管理員規則會將LLM使用者代理路由至Edge Optimize。 設定包含以下步驟：

**1. 設定路由準則 (使用者代理比對)**

設定下列使用者代理程式的路由:image.png

```
 *AdobeEdgeOptimize-AI*,
 *ChatGPT-User*,
 *GPTBot*,
 *OAI-SearchBot*,
 *PerplexityBot*,
 *Perplexity-User*
```

![設定路由準則](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. 設定來源和 SSL 行為**

將來源設為`live.edgeoptimize.net`並比對SAN至`*.edgeoptimize.net`

![Set 來源和 SSL 行為](/help/assets/optimize-at-edge/akamai-step2-origin.png)

**3. 設定快取金鑰變數**

將快取金鑰變數`PMUSER_EDGE_OPTIMIZE_CACHE_KEY`設定為`LLMCLIENT=TRUE;X_FORWARDED_HOST={{builtin.AK_HOST}}`

![設定快取金鑰變數](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

**4. 快取規則**

![快取規則](/help/assets/optimize-at-edge/akamai-step4-rules.png)

**5. 修改傳入要求標頭**

設定以下傳入要求標題：
`x-edgeoptimize-api-key`至從LLMO擷取的API金鑰
`x-edgeoptimize-config`至`LLMCLIENT=TRUE;`
`x-edgeoptimize-url`至`{{builtin.AK_URL}}`

![修改傳入要求標頭](/help/assets/optimize-at-edge/akamai-step5-request.png)

**6. 修改傳入回應標頭**

![修改傳入回應標頭](/help/assets/optimize-at-edge/akamai-step6-response.png)

**7. 快取 ID 修改**

![快取 ID 修改](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. 修改傳出要求標頭**

將`x-forwarded-host`標頭設為`{{builtin.AK_HOST}}`

![修改傳出要求標頭](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

**9. 網站容錯移轉**

![網站容錯移轉](/help/assets/optimize-at-edge/akamai-step9-failover.png)

![容錯移轉行為](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

![容錯移轉規則](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

網站容錯移轉可確保，如果Edge Optimize傳回`4XX`或`5XX`錯誤，請求會自動路由回您的預設來源，讓一般使用者仍會收到回應。

| 情境 | 行為 |
| --- | --- |
| Edge最佳化傳回`2XX` | 最佳化的回應會提供給使用者端。 |
| Edge最佳化傳回`4XX`或`5XX` | 要求會路由回預設來源。 |

{{verify-setup-byocdn}}

{{return-to-overview}}
