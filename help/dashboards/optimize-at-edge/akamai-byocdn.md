---
title: 在Edge最佳化 — Akamai (BYOCDN)
description: 瞭解如何在LLM Optimizer中為Edge最佳化設定Akamai BYOCDN。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 14%

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

「站台容錯移轉」設定包含兩個部分：容錯移轉行為（設定在主要的optimize-at-edge路由規則內）和個別的容錯移轉測試標頭規則。

**9a。 站台容錯移轉行為（在主要邊緣最佳化路由規則內）**

在主要路由規則內，設定「站台容錯移轉」行為和「進階XML」程式碼片段，如下所示：

![網站容錯移轉](/help/assets/optimize-at-edge/akamai-step9-failover.png)

透過進階XML新增值為`fo`的要求標頭`x-edgeoptimize-request`：

```
<forward:availability.fail-action2>
<add-header>
<status>on</status>
<name>x-edgeoptimize-request</name>
<value>fo</value>
</add-header>
</forward:availability.fail-action2>
```

![容錯移轉行為](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

**9b。 容錯移轉測試標頭規則（同層級規則）**

>[!IMPORTANT]
>
>將&#x200B;**EdgeOptimize Failover - Test Header**&#x200B;規則建立為路由規則的&#x200B;**同層級** （在相同層級） — **不是**&#x200B;巢狀在這些規則內。 在Akamai屬性管理員規則樹狀結構中，階層應該如下所示：
>
>```
>▼ Parent Rule
>   ▶ Optimize at Edge Routing     ← routing rule
>       EdgeOptimize Failover - Test Header       ← sibling, same level
>```
>
>這可確保容錯移轉測試標頭規則評估&#x200B;**所有**&#x200B;路由規則，而不僅僅一個。

如果要求標頭`x-edgeoptimize-request`值為`fo`，請將傳出回應標頭`x-edgeoptimize-fo`設定為`true`。

![容錯移轉規則](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

網站容錯移轉可確保，如果Edge Optimize傳回`4XX`或`5XX`錯誤，請求會自動路由回您的預設來源，讓一般使用者仍會收到回應。

| 情境 | 行為 |
| --- | --- |
| Edge最佳化傳回`2XX` | 最佳化的回應會提供給使用者端。 |
| Edge最佳化傳回`4XX`或`5XX` | 要求會路由回預設來源。 |

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

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

{{return-to-overview}}
