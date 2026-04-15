---
title: 邊緣最佳化：Akamai (BYOCDN)
description: 了解在 LLM Optimizer 中如何設定 Akamai BYOCDN 進行邊緣最佳化。
feature: Opportunities
source-git-commit: 66b058734597c378040e77a23a4023bed9273427
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 94%

---


# Akamai (BYOCDN)

此設定會將代理式流量 (來自 AI 機器人和 LLM 使用者代理的要求) 路由至 Edge Optimize 後端服務 (`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中尋找 `x-edgeoptimize-request-id` 標頭。

**先決條件**

設定 Akamai Property Manager 規則之前，請確定您具備以下條件：

* 您網域的 Akamai Property Manager 存取權。
* 已完成 LLM Optimizer 上線流程。
* 已經將內容傳遞網路記錄轉送至 LLM Optimizer。
* 從 LLM Optimizer 使用者介面擷取的 Edge Optimize API 金鑰。
* （選擇性）若要測試暫存路由，請參閱此頁面結尾的&#x200B;**選擇性：測試暫存主機名稱上的路由**。

{{retrieve-byocdn-api-key}}

**設定**

下列 Akamai Property Manager 規則將代理式 HTML 頁面流量路由至 Edge Optimize。 設定包含以下步驟：

**1. 設定路由準則 (使用者 AI 代理與 HTML 流量比對)**

設定下列使用者 AI 代理的路由：

```
 *AdobeEdgeOptimize-AI*
 *ChatGPT-User*
 *GPTBot*
 *OAI-SearchBot*
 *PerplexityBot*
 *Perplexity-User*
```

>[!NOTE]
>
>僅對代理式 HTML 頁面流量套用邊緣架構最佳化路由規則。 常見的設定是使用&#x200B;**副檔名**&#x200B;等要求端準則來比對 `html`，以及針對無副檔名頁面 URL 比對 `EMPTY_STRING`。 如果您的網站透過其他 URL 模式提供 HTML，或包含如 API 端點等無副檔名的非頁面路由，請使用其他路徑準則來精確調整此規則。

![設定路由準則](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. 設定來源和 SSL 行為**

將來源設為 `live.edgeoptimize.net`，而「對照 SAN 至」設為 `*.edgeoptimize.net`

>[!NOTE]
>
>如果新增邊緣最佳化規則後屬性啟用失敗，請檢查該規則是否使用與預設規則不同的原始伺服器 SSL 驗證模式。 若確實不同，請更新邊緣最佳化規則使其符合預設規則。 例如，若預設規則使用&#x200B;**平台設定**，此處也應使用&#x200B;**平台設定**。 如果您無法使用必要的設定，請聯絡 Akamai 支援。

![Set 來源和 SSL 行為](/help/assets/optimize-at-edge/akamai-step2-origin.png)

**3. 設定快取金鑰變數**

將快取鍵變數 `PMUSER_EDGE_OPTIMIZE_CACHE_KEY` 設為 `LLMCLIENT=TRUE;X_FORWARDED_HOST={{builtin.AK_HOST}}`

![設定快取金鑰變數](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

**4. 快取規則**

![快取規則](/help/assets/optimize-at-edge/akamai-step4-rules.png)

**5. 修改傳入要求標頭**

設定以下傳入要求標頭：
`x-edgeoptimize-api-key` 設定為從 LLMO 擷取的 API 金鑰
`x-edgeoptimize-config` 設定為 `LLMCLIENT=TRUE;`
`x-edgeoptimize-url` 設定為 `{{builtin.AK_URL}}`

![修改傳入要求標頭](/help/assets/optimize-at-edge/akamai-step5-request.png)

**允許透過防火牆規則在Edge最佳化（選用）**

{{waf-allowlist-setup}}

![在屬性管理員中設定x-edgeoptimize-fetcher-key標頭](/help/assets/optimize-at-edge/akamai-step10-fetcher-key.png)

>[!NOTE]
>
>同時將Akamai Bot Manager中的`*AdobeEdgeOptimize/1.0*`使用者代理程式和`x-edgeoptimize-fetcher-key`標頭加入允許清單。

**6. 修改傳入回應標頭**

![修改傳入回應標頭](/help/assets/optimize-at-edge/akamai-step6-response.png)

**7. 快取 ID 修改**

![快取 ID 修改](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. 修改傳出要求標頭**

將 `x-forwarded-host` 標頭設為 `{{builtin.AK_HOST}}`

![修改傳出要求標頭](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

**9. 網站容錯移轉**

網站容錯移轉設定包含兩個部分：容錯移轉行為 (於主要的邊緣最佳化路由規則內設定)，和個別的容錯移轉測試標頭規則。

**9a. 網站容錯移轉行為 (在主要的邊緣最佳化路由規則內)**

在主要路由規則內，設定網站容錯移轉行為和進階 XML 程式碼片段，如下所示：

>[!IMPORTANT]
>
>此步驟中的 XML 程式碼片段需要&#x200B;**進階**&#x200B;行為。 在某些 Akamai 環境中，此行為不適用自助式編輯。 如果沒有看到&#x200B;**進階**&#x200B;選項，請聯絡您的 Akamai 帳戶團隊或 Akamai 支援，啟用必要的設定。

![網站容錯移轉](/help/assets/optimize-at-edge/akamai-step9-failover.png)

透過進階 XML 新增值為 `fo` 的要求標頭 `x-edgeoptimize-request`：

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

**9b. 容錯移轉測試標頭規則 (同級規則)**

>[!IMPORTANT]
>
>建立 **EdgeOptimize 容錯移轉，測試標頭**&#x200B;規則作為路由規則的&#x200B;**同級** (位於相同層級) 規則，而&#x200B;**不是**&#x200B;以巢狀方式置於路由規則內。 在 Akamai Property Manager 規則樹狀結構中，階層應該如下所示：
>
>```
>▼ Parent Rule
>   ▶ Optimize at Edge Routing     ← routing rule
>       EdgeOptimize Failover - Test Header       ← sibling, same level
>```
>
>這樣能確保容錯移轉測試標頭規則針對&#x200B;**所有**&#x200B;路由規則，而非單一規則進行評估。
>
>同時請確保&#x200B;**邊緣最佳化路由**&#x200B;規則不會被任何後續的相符規則覆寫，以免後續規則變更相同請求的來源、快取行為或快取 ID。 如果另一個相符的規則重設這些行為，邊緣最佳化路由或快取可能無法如預期運作。

如果要求標頭 `x-edgeoptimize-request` 值為 `fo`，請將傳出回應標頭 `x-edgeoptimize-fo` 設定為 `true`。

![容錯移轉規則](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

透過網站容錯移轉，您可以確保如果 Edge Optimize 傳回 `4XX` 或 `5XX` 錯誤，該要求會自動路由回到您的預設來源，讓一般使用者仍能收到回應。

| 情境 | 行為 |
| --- | --- |
| Edge Optimize 傳回 `2XX` | 最佳化的回應會傳送至用戶端。 |
| Edge Optimize 傳回 `4XX` 或 `5XX` | 該要求路由回到預設來源。 |

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

{{verify-routing-status-in-ui}}

{{retrieve-staging-edge-optimize-api-key}}

```
curl -svo /dev/null https://staging.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

{{return-to-overview}}
