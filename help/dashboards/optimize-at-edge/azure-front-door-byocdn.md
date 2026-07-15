---
title: 在Edge最佳化 — Azure前門(BYOCDN)
description: 瞭解如何在LLM Optimizer中為Edge最佳化設定Azure前門BYOCDN。
feature: Opportunities
autotag-review: '2026-07-15T17:40:54.797Z'
TQID: 'https://experienceleague.adobe.com/fe-kultqzWQdRdcUjzfNs21UpL6m5zcoAmaQyMMv5kk'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72id: e1b649f0-0a61-46e4-9082-64d5cb2576c6id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3aid: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2: id: d23587d6-14d6-4e3f-9ee1-cc18623832e1id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 768
ht-degree: 24%

---


# Azure正門(BYOCDN)

此設定會將代理式流量 (來自 AI 機器人和 LLM 使用者代理的要求) 路由至 Edge Optimize 後端服務 (`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中尋找 `x-edgeoptimize-request-id` 標頭。

Azure Front Door不會在邊緣執行自訂程式碼。 路由設定是使用&#x200B;**規則集**&#x200B;以及專用的&#x200B;**原始群組**&#x200B;進行Edge最佳化。 容錯移轉由Azure Front Door的優先順序型原始群組健康狀態探查處理。

**先決條件**

設定Azure前門路由規則之前，請確定您擁有：

* 存取您的Azure前門設定檔。
* 從 LLM Optimizer 使用者介面擷取的 Edge Optimize API 金鑰。 相關步驟請參閱[檢索 API 金鑰](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* (選用) 若要測試中繼路由，請參閱[中繼 API 金鑰](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

## 步驟1：建立Edge最佳化的原始群組

您的Azure前門設定檔已有指向您原點的預設來源群組。 為Edge最佳化建立&#x200B;**新**&#x200B;原始群組：

* **名稱：**`edge-optimize-origin-group`
* **原點（優先順序容錯移轉）：**
   * **優先順序1** — `live.edgeoptimize.net` （原始主機標頭： `live.edgeoptimize.net`）
   * **優先順序2** — 您的網域端點（例如，`www.example.com`）。 這是用於容錯移轉：如果Edge Optimize狀況不良，會要求路由至您的網域，而網域會重新進入Azure前門，並從您的預設來源提供服務。
* **健康情況探查：** **已啟用**
   * 路徑： `/health/<your-domain>` （例如，`/health/www.example.com`）
   * 通訊協定： HTTPS
   * 間隔：225秒
* **工作階段相關性：** **已停用**
* **憑證主體名稱驗證：** **已啟用**

![Edge最佳化原始群組，包含兩個以優先順序為基礎的原始和健康狀態探查](/help/assets/optimize-at-edge/azure-front-door-origin-group.png)

>[!NOTE]
>
>`edge-optimize-origin-group`來源群組在入口網站中顯示&#x200B;**「未關聯」**&#x200B;警告。 這是預期的 — 它是透過「規則集」路由覆寫來參照，而非直接透過路由參照。

## 步驟2：設定路由

預設路由通常以您的Azure前門設定檔建立。 規則集（步驟3）會覆寫代理流量的來源群組，因此Edge Optimize不需要個別的路由。

## 步驟3：建立規則集

移至&#x200B;**規則集** > **新增規則集**&#x200B;並將其命名為`EORouting`。 依此順序新增三個規則。

![EORouting規則集顯示標題移除和機器人路由規則](/help/assets/optimize-at-edge/azure-front-door-ruleset-routing.png)

### 規則1： StripIncomingEOHeaders01

拆解傳入的Edge Optimize標題以防止詐騙。 無條件 — 適用於所有要求。 停止評估： **關閉**。

**動作** — 刪除每個的請求標頭：

* `x-edgeoptimize-url`
* `x-edgeoptimize-config`
* `x-edgeoptimize-api-key`
* `x-edgeoptimize-fetcher-key`

### 規則2： EOGPTBotRootGET03

在HTML頁面路徑上將機器人請求路由到Edge最佳化。 停止評估： **於**。

**條件** （所有條件都必須相符）：

* 要求方法： **等於** `GET`
* 要求路徑： **RegEx** `(^$|^.*/$|(^|.*/)[^./]+$|^.*\.html$)` （符合網站根目錄、以`/`結尾的路徑、無擴充功能的頁面路徑，以及`.html`個路徑）
* 使用者代理程式： **包含任何** `chatgpt-user`、`gptbot`、`oai-searchbot`、`adobeedgeoptimize-ai`、`perplexitybot`、`perplexity-user`、`claudebot`、`claude-user`、`claude-searchbot`。 將字串轉換設為&#x200B;**小寫**。
* `x-edgeoptimize-monitor`： **不包含** `1`
* `x-edgeoptimize-request`： **不包含任何** `failover`，`1`

**動作**：

* 要求標頭覆寫`x-edgeoptimize-url` = `/{url_path}?{query_string}`
* 要求標頭覆寫`x-edgeoptimize-config` = `LLMCLIENT=TRUE;`
* 要求標頭覆寫`x-edgeoptimize-api-key` = `YOUR_API_KEY`
* 要求標頭覆寫`x-edgeoptimize-monitor` = `1`
* 路由設定覆寫：原始群組→ `edge-optimize-origin-group`，轉送通訊協定→符合傳入要求，快取→ **已停用**

### 規則3： HealthProbeRewrite03

重寫Azure Front Door健康情況探查要求，以便以`/` （而非`/health/<domain>`）的身分到達您的來源。 這可讓Azure前門監控Edge最佳化可用性，而不需要來源上的專屬健全狀態端點。 停止評估： **於**。

![健康情況探查重寫規則](/help/assets/optimize-at-edge/azure-front-door-ruleset-healthprobe.png)

**條件** （所有條件都必須相符）：

* 要求URL路徑： **開頭為** `/health/`
* `x-fd-healthprobe`： **包含** `1`

**動作**：

* URL重寫 — Source模式： `/health/`，目的地： `/`
* 回應標頭覆寫`custom-origin-health` = `routed` （診斷 — 可在驗證後移除）
* 請求標題附加`user-agent` = ` AdobeEdgeOptimize/1.0` （新增前置空格 — Azure Front Door會依原樣附加值）
* 路由設定覆寫：原始群組→ `default-origin-group`，轉送通訊協定→符合傳入要求，快取→ **已停用**

## 步驟4：將規則集與路由關聯

開啟您的路由，捲動至底部的&#x200B;**規則**&#x200B;區段，然後從下拉式清單中選取`EORouting`規則集。 如果您有現有的規則集，請使用&#x200B;**移至頂端**&#x200B;以在&#x200B;**#1**&#x200B;定位`EORouting`。 Edge規則最佳化只會截獲代理流量，Edge最佳化回圈請求 — 所有其他流量都可不受影響地通過至您的其他規則。 儲存並等待傳播（大約20分鐘）。

## 允許透過防火牆規則在Edge最佳化（選用）

{{waf-allowlist-setup}}

## 驗證設定

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

{{verify-routing-status-in-ui}}

{{return-to-overview}}
