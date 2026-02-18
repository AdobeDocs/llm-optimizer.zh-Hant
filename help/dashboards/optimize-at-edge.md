---
title: 在Edge最佳化
description: 瞭解如何在CDN邊緣的LLM Optimizer中提供最佳化，而不需要任何編寫變更。
feature: Opportunities
source-git-commit: 1f665bd14349c15d92f8274742606abcf9b02000
workflow-type: tm+mt
source-wordcount: '4708'
ht-degree: 1%

---


# 在Edge最佳化

本頁提供如何在CDN邊緣傳遞最佳化，而不會變更編寫內容的詳細概觀。 內容包括入門流程、可用的最佳化機會，以及如何在Edge自動最佳化。

>[!NOTE]
>此功能目前正在搶先使用。 您可以[在此](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)進一步瞭解搶先存取計畫。

## 什麼是Edge的最佳化？

Edge最佳化是LLM Optimizer中的邊緣型部署功能，可為LLM使用者代理提供人工智慧易用的變更。 在目前的內容中，「Edge」表示最佳化會套用在CDN層。 由於它在CDN層提供最佳化，因此不需要在內容管理系統(CMS)中進行製作變更，因此您的原始CMS保持不變。 此分隔可讓您在不變更現有發佈工作流程的情況下改善LLM可見性。 它只會鎖定代理程式流量，不影響人類使用者或SEO機器人。 當LLM Optimizer偵測到最佳化頁面的機會時，使用者可以直接在CDN邊緣部署修正。

Edge最佳化是需要複雜工程作業的傳統修正的替代方案，而且速度更快、更精簡。 如前所述，完成一次性設定後，就不需要變更平台或延長開發週期即可套用變更。 您可以在幾分鐘內發佈改善功能，而不需要開發人員的參與。 這是針對AI代理最佳化您的網站的非程式碼方式。

Edge最佳化專為行銷、SEO、內容和數位策略團隊中的業務使用者設計。 這可讓業務使用者完成LLM Optimizer的完整歷程：識別機會、瞭解建議，並輕鬆部署修正。 透過Edge中的最佳化，使用者可以預覽變更、在CDN邊緣快速部署變更，以及驗證最佳化是否即時。 可在LLM Optimizer生態系統中追蹤效能。

### 主要優點

* **僅限AI的傳遞：**&#x200B;僅將最佳化的HTML提供給AI代理程式，不會對人類訪客或SEO機器人造成影響。
* **更快的週期：**&#x200B;在幾分鐘內發佈變更，而不是幾週。 不需要變更平台或延長工程週期。
* **可回覆：**&#x200B;支援一鍵回覆功能，可在數分鐘內回覆頁面。
* **沒有效能影響：**&#x200B;以Edge為基礎的最佳化和快取，讓網站延遲不受影響。
* **CDN與CMS無關：**&#x200B;可與任何CDN設定和前端設定搭配使用，無論內容管理系統為何。

### Edge的最佳化支援哪些機會？

Edge的「最佳化」可支援改善代理程式網頁體驗的商機。 在[機會儀表板](/help/dashboards/opportunities.md)頁面和目前頁面的機會區段中，進一步瞭解每個機會。

## 上線

您應該聯絡您的Adobe客戶團隊或FDE團隊以開始入門流程。 您的IT或CDN團隊也必須完成先決條件和設定程式。 此外，您也可以聯絡`llmo-at-edge@adobe.com`以取得進一步的入門協助。

在Edge上線最佳化的先決條件：

* 完成LLM Optimizer的上線流程。
* 完成CDN記錄檔的記錄檔轉送程式。

您的IT/CDN團隊需求：
* 將`*AdobeEdgeOptimize/1.0*`使用者代理程式新增至您網站的robots.txt檔案或bot-traffic管理規則中的允許清單。
* 請確定在網域或CDN層級未封鎖頁面。
* 在CDN中新增Edge路由規則最佳化。
* 在LLM Optimizer介面中確認Edge路由最佳化。

為引導設定流程，以下提供若干CDN設定的設定範例。 請記住，這些範例應該根據您的實際即時設定進行調整。 我們建議先在較低層級環境中套用變更。

>[!BEGINTABS]

>[!TAB AEM Cloud Service Managed CDN (Fastly)]

**Edge最佳化 — AEM Cloud Service Managed CDN (Fastly)**

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

如果您偏好透過Cloud Manager Pipeline自行設定繞線，請遵循下列步驟。 路由設定是使用[originSelector CDN規則](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)完成。 先決條件如下：

* 決定要路由的網域。
* 決定要繞線的路徑。
* 決定要路由的使用者代理程式（建議的規則運算式）。

若要部署規則，您需要：

* 建立[設定管道](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/config-pipeline)。
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

若要測試設定，請執行curl並預期出現以下情況：

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

流量路由的狀態也可以在LLM Optimizer UI中檢視。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

>[!TAB Fastly (BYOCDN)]

**Edge最佳化 — Fastly (BYOCDN)**

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

在設定Fastly VCL規則之前，請確定您具有：

* 為您的網域存取Fastly。
* 已完成LLM Optimizer上線程式。
* 已完成CDN記錄檔轉送至LLM Optimizer。
* 從Edge UI擷取的LLM Optimizer最佳化API金鑰。

**擷取API金鑰的步驟：**

1. 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

   ![瀏覽至客戶組態](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**部署最佳化的AI流量路由**&#x200B;下，勾選&#x200B;**部署最佳化至AI代理程式**&#x200B;核取方塊。

   ![勾選部署最佳化至AI代理程式](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 複製API金鑰，並繼續執行下列路由設定步驟。

   ![複製API金鑰](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >在此階段，狀態可能顯示紅叉，表示設定尚未完成。 這是預期中的情形 — 當您完成下方的路由設定，且AI機器人流量開始流動時，狀態將更新為綠色核取記號，確認路由已成功啟用。

此外，如果您需要上述步驟的任何協助，請聯絡您的Adobe帳戶團隊或`llmo-at-edge@adobe.com`。

**組態**

將下列三個VCL片段新增至您的Fastly服務。 這些程式碼片段會處理路由代理對Edge Optimize、快取金鑰分離和容錯移轉至預設來源的請求。

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![新增VCL程式碼片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv程式碼片段**

```
unset req.http.x-edgeoptimize-url;
unset req.http.x-edgeoptimize-config;
unset req.http.x-edgeoptimize-api-key;

if (!req.http.x-edgeoptimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-forwarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edgeoptimize-url = req.url; # required for identifying the original url
  set req.http.x-edgeoptimize-config = "LLMCLIENT=TRUE;"; # required for cache key
  set req.http.x-edgeoptimize-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_EDGE_OPTIMIZE;
}
```

**vcl_hash程式碼片段**

```
if (req.http.x-edgeoptimize-config) {
  set req.hash += "edge-optimize";
  set req.hash += req.http.x-edgeoptimize-config;
}
```

**vcl_deliver程式碼片段**

```
if (req.http.x-edgeoptimize-config && resp.status >= 400) {
  set req.http.x-edgeoptimize-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-edgeoptimize-config && req.http.x-edgeoptimize-request == "failover") {
  set resp.http.x-edgeoptimize-fo = "1";
}
```

**容錯移轉**

`vcl_deliver`程式碼片段會自動處理容錯移轉。 如果Edge Optimize傳回`4XX`或`5XX`錯誤，要求會重新啟動並路由回您的預設來源，讓一般使用者仍會收到回應。 容錯移轉回應包含`x-edgeoptimize-fo: 1`標頭。

| 情境 | 行為 |
| --- | --- |
| Edge最佳化傳回`2XX` | 最佳化的回應會提供給使用者端。 |
| Edge最佳化傳回`4XX`或`5XX` | 系統會重新啟動要求，並從預設來源提供服務。 |
| 容錯移轉回應 | 包含標頭`x-edgeoptimize-fo: 1`。 |

**驗證安裝程式**

若要測試設定，請執行curl並預期出現以下情況：

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

流量路由的狀態也可以在LLM Optimizer UI中檢視。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

>[!TAB Akamai (BYOCDN)]

**Edge最佳化 — Akamai (BYOCDN)**

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

在設定Akamai屬性管理員規則之前，請確定您已：

* 存取您網域的Akamai屬性管理員。
* 已完成LLM Optimizer上線程式。
* 已完成CDN記錄檔轉送至LLM Optimizer。
* 從Edge UI擷取的LLM Optimizer最佳化API金鑰。

**擷取API金鑰的步驟：**

1. 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

   ![瀏覽至客戶組態](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**部署最佳化的AI流量路由**&#x200B;下，勾選&#x200B;**部署最佳化至AI代理程式**&#x200B;核取方塊。

   ![勾選部署最佳化至AI代理程式](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 複製API金鑰，並繼續執行下列路由設定步驟。

   ![複製API金鑰](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >在此階段，狀態可能顯示紅叉，表示設定尚未完成。 這是預期中的情形 — 當您完成下方的路由設定，且AI機器人流量開始流動時，狀態將更新為綠色核取記號，確認路由已成功啟用。

此外，如果您需要上述步驟的任何協助，請聯絡您的Adobe帳戶團隊或`llmo-at-edge@adobe.com`。

**組態**

以下Akamai屬性管理員規則會將LLM使用者代理路由至Edge Optimize。 此設定包括下列步驟：

**1. 設定路由條件（使用者 — 代理程式比對）**

設定下列使用者代理程式的路由：

```
 *AdobeEdgeOptimize-AI*,
 *ChatGPT-User*,
 *GPTBot*,
 *OAI-SearchBot*,
 *PerplexityBot*,
 *Perplexity-User*
```

![設定路由條件](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. 設定來源和SSL行為**

將來源設為`live.edgeoptimize.net`並比對SAN至`*.edgeoptimize.net`

![設定來源和SSL行為](/help/assets/optimize-at-edge/akamai-step2-origin.png)

**3. 設定快取金鑰變數**

將快取金鑰變數`PMUSER_EDGE_OPTIMIZE_CACHE_KEY`設定為`LLMCLIENT=TRUE;X_FORWARDED_HOST={{builtin.AK_HOST}}`

![設定快取金鑰變數](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

**4. 快取規則**

![快取規則](/help/assets/optimize-at-edge/akamai-step4-rules.png)

**5. 修改傳入的要求標頭**

設定以下傳入要求標題：
`x-edgeoptimize-api-key`至從LLMO擷取的API金鑰
`x-edgeoptimize-config`至`LLMCLIENT=TRUE;`
`x-edgeoptimize-url`至`{{builtin.AK_URL}}`

![修改傳入的要求標頭](/help/assets/optimize-at-edge/akamai-step5-request.png)

**6. 修改傳入的回應標題**

![修改傳入的回應標題](/help/assets/optimize-at-edge/akamai-step6-response.png)

**7. 快取識別碼修改**

![快取ID修改](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. 修改傳出要求標頭**

將`x-forwarded-host`標頭設為`{{builtin.AK_HOST}}`

![修改傳出要求標頭](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

**9. 站台容錯移轉**

![站台容錯移轉](/help/assets/optimize-at-edge/akamai-step9-failover.png)

![容錯移轉行為](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

![容錯移轉規則](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

網站容錯移轉可確保，如果Edge Optimize傳回`4XX`或`5XX`錯誤，請求會自動路由回您的預設來源，讓一般使用者仍會收到回應。

| 情境 | 行為 |
| --- | --- |
| Edge最佳化傳回`2XX` | 最佳化的回應會提供給使用者端。 |
| Edge最佳化傳回`4XX`或`5XX` | 要求會路由回預設來源。 |

**驗證安裝程式**

若要測試設定，請執行curl並預期出現以下情況：

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

流量路由的狀態也可以在LLM Optimizer UI中檢視。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

>[!TAB Cloudflare (BYOCDN)]

**Edge最佳化 — Cloudflare (BYOCDN)**

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

在設定Cloudflare Worker路由規則之前，請確定您具有：

* 在您的網域中啟用Worker的Cloudflare帳戶。
* 在Cloudflare中存取您網域的DNS設定。
* 已完成LLM Optimizer上線程式。
* 已完成CDN記錄檔轉送至LLM Optimizer。
* 從Edge UI擷取的LLM Optimizer最佳化API金鑰。

**擷取API金鑰的步驟：**

1. 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

   ![瀏覽至客戶組態](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**部署最佳化的AI流量路由**&#x200B;下，勾選&#x200B;**部署最佳化至AI代理程式**&#x200B;核取方塊。

   ![勾選部署最佳化至AI代理程式](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 複製API金鑰，並繼續執行下列路由設定步驟。

   ![複製API金鑰](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >在此階段，狀態可能顯示紅叉，表示設定尚未完成。 這是預期中的情形 — 當您完成下方的路由設定，且AI機器人流量開始流動時，狀態將更新為綠色核取記號，確認路由已成功啟用。

此外，如果您需要上述步驟的任何協助，請聯絡您的Adobe帳戶團隊或`llmo-at-edge@adobe.com`。

**路由如何運作**

正確設定後，Cloudflare Worker會攔截代理使用者代理對您網域（例如`www.example.com/page.html`）的請求，並將其路由至Edge最佳化後端。 後端請求包含必要的標頭。

**正在測試後端要求**

您可以透過直接向Edge最佳化後端提出請求來驗證路由。

```
curl -svo /dev/null https://live.edgeoptimize.net/page.html \
  -H 'x-forwarded-host: www.example.com' \
  -H 'x-edgeoptimize-url: /page.html' \
  -H 'x-edgeoptimize-api-key: $EDGE_OPTIMIZE_API_KEY' \
  -H 'x-edgeoptimize-config: LLMCLIENT=TRUE;'
```

**必要的標頭**

對Edge最佳化後端的請求必須設定以下標頭：

| 頁首 | 說明 | 範例 |
|--------|-------------|---------|
| `x-forwarded-host` | 要求的原始主機。 識別網站網域時需要。 | `www.example.com` |
| `x-edgeoptimize-url` | 請求的原始URL路徑和查詢字串。 | `/page.html` 或 `/products?id=123` |
| `x-edgeoptimize-api-key` | Adobe為您的網域提供的API金鑰。 | `your-api-key-here` |
| `x-edgeoptimize-config` | 快取金鑰差異的設定字串。 | `LLMCLIENT=TRUE;` |

**步驟1：建立Cloudflare Worker**

1. 登入您的Cloudflare儀表板。
2. 在側邊欄中導覽至&#x200B;**工作者與頁面**。
3. 按一下&#x200B;**建立應用程式**，然後按一下&#x200B;**建立背景工作**。
4. 為您的背景工作命名（例如，`edge-optimize-router`）。
5. 按一下&#x200B;**部署**&#x200B;以使用預設程式碼建立背景工作。

![Cloudflare Worker儀表板](/help/assets/optimize-at-edge/cloudflare-workers-dashboard.png)

**步驟2：新增背景工作程式碼**

建立背景工作之後，按一下&#x200B;**編輯程式碼**，然後以下列專案取代預設程式碼：

```javascript
/**
 * Edge Optimize BYOCDN - Cloudflare Worker
 *
 * This worker routes requests from agentic bots (AI/LLM user agents) to the
 * Edge Optimize backend service for optimized content delivery.
 *
 * Features:
 * - Routes agentic bot traffic to Edge Optimize backend
 * - Failover to origin on Edge Optimize errors (any 4XX or 5XX errors)
 * - Loop protection to prevent infinite redirects
 * - Human visitors and SEO bots are served from the origin as usual
 */

// List of agentic bot user agents to route to Edge Optimize
const AGENTIC_BOTS = [
  'AdobeEdgeOptimize-AI',
  'ChatGPT-User',
  'GPTBot',
  'OAI-SearchBot',
  'PerplexityBot',
  'Perplexity-User'
];

// Targeted paths for Edge Optimize routing
// Set to null to route all HTML pages, or specify an array of paths
const TARGETED_PATHS = null; // e.g., ['/', '/page.html', '/products']

// Failover configuration
// Failover on any 4XX client error or 5XX server error from Edge Optimize
const FAILOVER_ON_4XX = true; // Failover on any 4XX error (400-499)
const FAILOVER_ON_5XX = true; // Failover on any 5XX error (500-599)

export default {
  async fetch(request, env, ctx) {
    return await handleRequest(request, env, ctx);
  },
};

async function handleRequest(request, env, ctx) {
  const url = new URL(request.url);
  const userAgent = request.headers.get("user-agent")?.toLowerCase() || "";

  // Check if request is already processed (loop protection)
  const isEdgeOptimizeRequest = !!request.headers.get("x-edgeoptimize-request");

  // Construct the original path and query string
  const pathAndQuery = `${url.pathname}${url.search}`;

  // Check if the path matches HTML pages (no extension or .html extension)
  const isHtmlPage = /(?:\/[^./]+|\.html|\/)$/.test(url.pathname);

  // Check if path is in targeted paths (if specified)
  const isTargetedPath = TARGETED_PATHS === null
    ? isHtmlPage
    : (isHtmlPage && TARGETED_PATHS.includes(url.pathname));

  // Check if user agent is an agentic bot
  const isAgenticBot = AGENTIC_BOTS.some((ua) => userAgent.includes(ua.toLowerCase()));

  // Route to Edge Optimize if:
  // 1. Request is NOT already from Edge Optimize (prevents infinite loops)
  // 2. User agent matches one of the agentic bots
  // 3. Path is targeted for optimization
  if (!isEdgeOptimizeRequest && isAgenticBot && isTargetedPath) {

    // Build the Edge Optimize request URL
    const edgeOptimizeURL = `https://live.edgeoptimize.net${pathAndQuery}`;

    // Clone and modify headers for the Edge Optimize request
    const edgeOptimizeHeaders = new Headers(request.headers);

    // Remove any existing Edge Optimize headers for security
    edgeOptimizeHeaders.delete("x-edgeoptimize-api-key");
    edgeOptimizeHeaders.delete("x-edgeoptimize-url");
    edgeOptimizeHeaders.delete("x-edgeoptimize-config");

    // x-forwarded-host: The original site domain
    // Use environment variable if set, otherwise use the request host
    edgeOptimizeHeaders.set("x-forwarded-host", env.EDGE_OPTIMIZE_TARGET_HOST ?? url.host);

    // x-edgeoptimize-api-key: Your Adobe-provided API key
    edgeOptimizeHeaders.set("x-edgeoptimize-api-key", env.EDGE_OPTIMIZE_API_KEY);

    // x-edgeoptimize-url: The original request URL path and query
    edgeOptimizeHeaders.set("x-edgeoptimize-url", pathAndQuery);

    // x-edgeoptimize-config: Configuration for cache key differentiation
    edgeOptimizeHeaders.set("x-edgeoptimize-config", "LLMCLIENT=TRUE;");

    try {
      // Send request to Edge Optimize backend
      const edgeOptimizeResponse = await fetch(new Request(edgeOptimizeURL, {
        headers: edgeOptimizeHeaders,
        redirect: "manual", // Preserve redirect responses from Edge Optimize
      }), {
        cf: {
          cacheEverything: true, // Enable caching based on origin's cache-control headers
        },
      });

      // Check if we need to failover to origin
      const status = edgeOptimizeResponse.status;
      const is4xxError = FAILOVER_ON_4XX && status >= 400 && status < 500;
      const is5xxError = FAILOVER_ON_5XX && status >= 500 && status < 600;

      if (is4xxError || is5xxError) {
        console.log(`Edge Optimize returned ${status}, failing over to origin`);
        return await failoverToOrigin(request, env, url);
      }

      // Return the Edge Optimize response
      return edgeOptimizeResponse;

    } catch (error) {
      // Network error or timeout - failover to origin
      console.log(`Edge Optimize request failed: ${error.message}, failing over to origin`);
      return await failoverToOrigin(request, env, url);
    }
  }

  // For all other requests (human users, SEO bots), pass through unchanged
  return fetch(request);
}

/**
 * Failover to origin server when Edge Optimize returns an error
 * @param {Request} request - The original request
 * @param {Object} env - Environment variables
 * @param {URL} url - Parsed URL object
 */
async function failoverToOrigin(request, env, url) {
  // Build origin URL
  const originURL = `${url.protocol}//${env.EDGE_OPTIMIZE_TARGET_HOST}${url.pathname}${url.search}`;

  // Prepare headers - clean Edge Optimize headers and add loop protection
  const originHeaders = new Headers(request.headers);
  originHeaders.set("Host", env.EDGE_OPTIMIZE_TARGET_HOST);
  originHeaders.delete("x-edgeoptimize-api-key");
  originHeaders.delete("x-edgeoptimize-url");
  originHeaders.delete("x-edgeoptimize-config");
  originHeaders.delete("x-forwarded-host");
  originHeaders.set("x-edgeoptimize-request", "fo");

  // Create and send origin request
  const originRequest = new Request(originURL, {
    method: request.method,
    headers: originHeaders,
    body: request.body,
    redirect: "manual",
  });

  const originResponse = await fetch(originRequest);

  // Add failover marker header to response
  const modifiedResponse = new Response(originResponse.body, {
    status: originResponse.status,
    statusText: originResponse.statusText,
    headers: originResponse.headers,
  });
  modifiedResponse.headers.set("x-edgeoptimize-fo", "1");
  return modifiedResponse;
}
```

按一下&#x200B;**儲存並部署**&#x200B;以發佈背景工作。

![Cloudflare Worker程式碼編輯器](/help/assets/optimize-at-edge/cloudflare-worker-editor.png)

**步驟3：設定環境變數**

環境變數會安全地儲存敏感設定，例如您的API金鑰。

1. 在您的背景工作設定中，瀏覽至&#x200B;**設定** > **變數**。
2. 在&#x200B;**環境變數**&#x200B;下，按一下&#x200B;**新增變數**。
3. 新增下列變數：

   | 變數名稱 | 說明 | 必要 |
   |---------------|-------------|----------|
   | `EDGE_OPTIMIZE_API_KEY` | Adobe提供的Edge最佳化API金鑰。 | 是 |
   | `EDGE_OPTIMIZE_TARGET_HOST` | Edge最佳化請求的目標主機（以`x-forwarded-host`標題傳送）以及容錯移轉的原始網域。 必須是沒有通訊協定的網域（例如，`www.example.com`，而不是`https://www.example.com`）。 | 是 |

4. 對於API金鑰，按一下&#x200B;**加密**&#x200B;以安全地儲存。
5. 按一下&#x200B;**儲存並部署**。

![Cloudflare環境變數](/help/assets/optimize-at-edge/cloudflare-env-variables.png)

**步驟4：新增路由至您的網域**

若要在您的網域上啟用背景工作：

1. 前往工作者的&#x200B;**設定** > **觸發器**。
2. 在&#x200B;**路由**&#x200B;下，按一下&#x200B;**新增路由**。
3. 輸入您的網域模式（例如，`www.example.com/*`或`example.com/*`）。
4. 從下拉式清單中選取您的區域。
5. 按一下&#x200B;**儲存**。

或者，您可以在區域層級配置路由：

1. 在Cloudflare中導覽至您的網域。
2. 移至&#x200B;**工作者路由**。
3. 按一下&#x200B;**新增路由**&#x200B;並指定模式和工作者。

![Cloudflare背景工作路由](/help/assets/optimize-at-edge/cloudflare-worker-routes.png)

**步驟5：驗證設定**

部署後，請使用代理使用者代理程式提出要求以測試設定。

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的回應包含`x-edgeoptimize-request-id`標頭：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

流量路由的狀態也可以在LLM Optimizer UI中檢視。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

您也可以驗證正常的流量是否持續運作：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0"
```

此請求應該由您的來源提供，不含`x-edgeoptimize-request-id`標頭。

**正在驗證容錯移轉行為**

如果Edge Optimize無法使用或傳回錯誤，背景工作會自動容錯移轉至您的來源。 容錯移轉回應包含`x-edgeoptimize-fo`標頭：

```
< HTTP/2 200
< x-edgeoptimize-fo: 1
```

您可以在Cloudflare Worker記錄中監視容錯移轉事件，以疑難排解問題。

**瞭解背景工作邏輯**

Cloudflare Worker會實作下列邏輯：

1. **使用者代理程式偵測：**&#x200B;檢查傳入要求的使用者代理程式是否與任何已定義的代理程式機器人相符（不區分大小寫）。

2. **路徑目標定位：**&#x200B;選擇性地根據目標路徑篩選請求。 依預設，會路由所有HTML頁面（以`/`、無副檔名或`.html`結尾的URL）。 您可以使用`TARGETED_PATHS`陣列來指定特定路徑。

3. **回圈保護：** `x-edgeoptimize-request`標頭防止無限回圈。 當Edge Optimize將請求傳回您的來源時，此標題設為`"1"`，而背景工作傳遞請求時，不會將其路由回Edge Optimize。

4. **標頭安全性：**&#x200B;在設定Edge Optimize標頭之前，背景工作會從傳入要求中移除任何現有的`x-edgeoptimize-*`標頭，以避免標頭插入攻擊。

5. **標頭對應：**&#x200B;背景工作設定Edge最佳化所需的標頭：
   * `x-forwarded-host` — 識別原始網站網域。
   * `x-edgeoptimize-url` — 保留原始請求路徑和查詢字串。
   * `x-edgeoptimize-api-key` — 使用Edge Optimize驗證請求。
   * `x-edgeoptimize-config` — 提供快取金鑰組態。

6. **容錯移轉邏輯：**&#x200B;如果Edge Optimize傳回任何錯誤狀態碼（4XX使用者端錯誤或5XX伺服器錯誤），或要求因網路錯誤而失敗，背景工作程式會使用`EDGE_OPTIMIZE_TARGET_HOST`自動容錯移轉至您的來源。 容錯移轉回應包含`x-edgeoptimize-fo: 1`標頭，以指出已發生容錯移轉。

7. **重新導向處理：** `redirect: "manual"`選項可確保來自Edge Optimize的重新導向回應傳遞至使用者端，而不需要背景工作程式加以追蹤。

**自訂組態**

您可以修改程式碼頂端的設定常數，以自訂背景工作者的行為：

**代理程式機器人清單**

修改`AGENTIC_BOTS`陣列以新增或移除使用者代理程式：

```javascript
const AGENTIC_BOTS = [
  'AdobeEdgeOptimize-AI',
  'ChatGPT-User',
  'GPTBot',
  'OAI-SearchBot',
  'PerplexityBot',
  'Perplexity-User',
  // Add additional user agents as needed
  'ClaudeBot',
  'Anthropic-AI'
];
```

**目標路徑**

依預設，所有HTML頁面都會路由至Edge最佳化。 若要將路由限制在特定路徑，請修改`TARGETED_PATHS`陣列：

```javascript
// Route all HTML pages (default)
const TARGETED_PATHS = null;

// Or specify exact paths to route
const TARGETED_PATHS = ['/', '/page.html', '/products', '/about-us'];
```

**容錯移轉設定**

根據預設，背景工作會在Edge Optimize的任何4XX或5XX錯誤上容錯移轉。 自訂此行為：

```javascript
// Default: failover on any 4XX or 5XX error
const FAILOVER_ON_4XX = true;
const FAILOVER_ON_5XX = true;

// Failover only on 5XX server errors (not 4XX client errors)
const FAILOVER_ON_4XX = false;
const FAILOVER_ON_5XX = true;

// Disable automatic failover (not recommended)
const FAILOVER_ON_4XX = false;
const FAILOVER_ON_5XX = false;
```

**重要考量**

* **容錯移轉行為：**&#x200B;如果Edge Optimize傳回任何錯誤（4XX或5XX狀態代碼），或請求因網路錯誤而失敗，背景工作會自動容錯移轉至您的來源。 容錯移轉使用`EDGE_OPTIMIZE_TARGET_HOST`作為原始網域（類似Fastly的`F_Default_Origin`或CloudFront的`Default_Origin`）。 容錯移轉回應包含`x-edgeoptimize-fo: 1`標頭，可用於監視和偵錯。

* **快取：** Cloudflare預設會根據URL快取回應。 由於代理流量接收的內容與人為流量不同，請確保您的快取設定為此負責。 請考慮使用快取API或快取標頭來區分快取的內容。 `x-edgeoptimize-config`標頭應包含在您的快取金鑰中。

* **速率限制：**&#x200B;監視您的Edge Optimize使用，並視需要考慮對代理流量實作速率限制。

* **測試：**&#x200B;部署至生產環境之前，請一律在中繼環境中測試組態。 確認代理和人力流量皆如預期般運作。 藉由模擬Edge最佳化錯誤來測試容錯移轉行為。

* **記錄：**&#x200B;啟用Cloudflare Workers記錄以監視要求並疑難排解問題。 導覽至&#x200B;**背景工作** > **您的背景工作** > **記錄檔**&#x200B;以檢視即時記錄檔。 背景工作記錄容錯移轉事件以供偵錯。

**疑難排解**

| 問題 | 可能的原因 | 解決方案 |
|-------|----------------|----------|
| 回應中沒有`x-edgeoptimize-request-id`標頭 | 工作者路由不相符，或使用者代理不在代理程式機器人清單中。 | 確認您的路由模式符合請求URL。 檢查使用者代理程式是否在`AGENTIC_BOTS`陣列中。 |
| Edge最佳化中的401或403錯誤 | API金鑰無效或遺失。 | 驗證已在環境變數中正確設定`EDGE_OPTIMIZE_API_KEY`。 聯絡Adobe確認您的API金鑰為作用中。 |
| 無限重新導向或回圈 | 未正確設定或檢查回圈保護標頭。 | 請確定`x-edgeoptimize-request`標頭檢查已準備就緒。 |
| 受影響的人員流量 | 背景工作路由邏輯太廣泛。 | 驗證使用者代理程式比對邏輯正確且不區分大小寫。 檢查`TARGETED_PATHS`是否已正確設定。 |
| 回應時間緩慢 | Edge最佳化後端的網路延遲。 | 這是第一個請求的預期情況；後續請求會快取至Edge Optimize。 |
| 回應中的`x-edgeoptimize-fo: 1`標頭 | Edge最佳化傳回錯誤，並容錯移轉至來源。 | 檢查Cloudflare Worker記錄檔中的特定錯誤碼。 使用Adobe驗證Edge最佳化服務狀態。 |
| 容錯移轉無法運作 | 容錯移轉旗標已停用，或容錯移轉邏輯發生錯誤。 | 驗證`FAILOVER_ON_4XX`和`FAILOVER_ON_5XX`是否設定為`true`。 檢查工作者記錄檔中的錯誤訊息。 |
| 某些路徑未最佳化 | 路徑不符合目標路徑或HTML頁面模式。 | 驗證路徑是否在`TARGETED_PATHS` （若已指定）中，並符合HTML頁面規則運算式模式。 |
| 請求因無效的主機而失敗 | `EDGE_OPTIMIZE_TARGET_HOST`包含通訊協定（例如，`https://`）。 | 只使用沒有通訊協定的網域名稱（例如，`example.com`，而非`https://example.com`）。 |
| 容錯移轉期間發生530錯誤 | Cloudflare無法連線到來源，或容錯移轉要求有無效的標頭。 | 確認容錯移轉功能已移除Edge Optimize標頭。 請確認您的來源可存取，且DNS已正確設定。 |

>[!ENDTABS]

>[!NOTE]
>若是其他CDN提供者，請聯絡`llmo-at-edge@adobe.com`以協助您的IT/CDN團隊完成入門。 完成設定後，您可以在LLM Optimizer中部署最佳化Edge機會的建議。

## 機會

下表顯示可改善一般網頁體驗並受Edge最佳化支援的機會。

| 機會 | 類型 | 自動識別 | 自動建議 | 自動最佳化 |
|---------|----------|----------|----------|----------|
| 復原內容能見度 | 技術性 GEO | 偵測對AI代理程式隱藏關鍵內容的頁面。 顯示受影響的URL和可復原的預期內容。 | 強調指出可供AI代理使用的內容，並建議啟用這些頁面的預先呈現。 | 提供完整演算、適合AI使用的HTML快照，以代理流量復原先前隱藏的內容。 |
| 新增LLM易記摘要 | 內容最佳化 | 會識別在頁面或區段層級缺乏簡要摘要的長頁面或複雜頁面，讓AI難以快速掃描和理解。 | 建議在擷取關鍵內容的頁面和區段層級，使用AI產生的簡短摘要。 | 將摘要插入相關的HTML區段，改善模型解譯和說明頁面內容的方式。 |
| 新增相關常見問答 | 內容最佳化 | 偵測現有頁面內容中可能受益於常見問答集的意圖差距。 | 根據使用者意圖和現有主題建議AI產生的常見問題集內容。 | 將常見問題集內容插入HTML，讓頁面在AI導向的答案中更易發現且相關。 |
| 簡化複雜內容 | 內容最佳化 | 標示可能妨礙AI理解的具有複雜文字的頁面。 | 提供AI產生的複雜文字的簡化版本，同時保留原始含義。 | 重寫頁面中的複雜區段，改善AI可讀性。 |

### 其他工具

[Adobe LLM Optimizer：您的網頁是否可編輯？](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome擴充功能會顯示LLM可存取的網頁內容量，以及隱藏的專案。 此工具是免費獨立診斷工具，不需要產品授權或設定。

只要按一下，您就可以評估任何網站的電腦可讀性。 您可以檢視AI代理程式檢視的內容與人類使用者檢視內容的並排比較，並估計使用LLM Optimizer可復原的內容量。 檢視[AI可以讀取您的網站嗎？](https://business.adobe.com/blog/introducing-the-llm-optimizer-chrome-extension) 頁面以取得詳細資訊。

## 機會詳細資料

在接下來的章節中，您可以檢視Edge最佳化支援的每個商機的其他詳細資料。

### 復原內容能見度

此機會會標籤因使用者端轉譯而為AI代理隱藏關鍵內容的頁面。 對於每個已識別的頁面，它都會精確顯示AI代理程式檢視中缺少哪些內容、醒目提示可見度差距，並可讓您直接套用變更以復原隱藏的內容。 當您透過Edge的「最佳化」部署此商機時，系統會為LLM使用者代理程式提供預先轉譯的AI最佳化頁面版本，讓他們無需執行Javascript即可存取完整內容。
這可確保頁面首先對AI代理完全可見。 在該預先轉譯的HTML上套用其他增強功能。

>[!IMPORTANT]
>此預先呈現功能在Edge部署最佳化時自動套用至下面顯示的所有機會，以確保AI代理程式完全可看見頁面。

### 新增LLM易記摘要

這個機會可辨識哪些頁面可受益於簡潔的摘要，以協助LLM快速瞭解頁面內容的內容。 對於每個頁面，機會會偵測最需要摘要的位置，並在頁面層級或區段層級建立AI產生的摘要。 使用Edge最佳化進行部署時，這些摘要會插入AI代理程式擷取的HTML中，進而提高更準確地描述您內容的機會。

### 新增相關常見問答

此機會會標籤頁面，指出其他問答內容可以更好地匹配使用者意圖和AI驅動探索中的提示。 對於每個頁面，它會建議與頁面上的使用者意圖和內容相連結的AI產生的常見問題集區塊。 透過Edge最佳化，這些常見問題集會插入到HTML中，讓您的頁面更適合AI，並增加AI回答直接反映您指引的可能性。

### 簡化複雜內容

這個機會會尋找具有長且複雜段落的頁面，這些段落可能會降低AI理解。 對於超過可讀性臨界值的每個頁面，它會建立AI產生的內容，這些內容更簡單、更易於掃描，同時保留原始含義。 部署於邊緣時，傳送至代理流量的簡化內容可協助LLM更忠實地解讀及摘要您的內容。

## 在Edge自動最佳化

您可以針對每個機會，在邊緣預覽、編輯、部署、檢視即時和復原最佳化。

>[!VIDEO](https://video.tv.adobe.com/v/3477983/?learn=on&enablevpops)

### 預覽

**預覽**&#x200B;可讓您在建議上線之前檢視建議的影響。 它會顯示目前頁面與套用建議後預期的AI最佳化版本之間的並排差異。 此檢視使用與支援即時流量的Edge最佳化邏輯相同的邏輯，但處於隔離的預覽模式。 這不會影響即時流量，因為這是供檢閱的唯讀模擬。

![預覽](/help/assets/optimize-at-edge/preview.png)

### 編輯

**編輯**&#x200B;可讓您在部署自動產生的建議之前，先調整或完全重寫這些建議。 您不必接受建議，而是透過編輯工作流程保持完全控制權。 檢視會在結構化編輯器中顯示提議的變更，您可以在此修改文字以更符合您的原始意圖。 部署後，編輯的版本將傳送給AI代理程式。

![編輯](/help/assets/optimize-at-edge/edit.png)

### 部署

**部署**&#x200B;會發佈選取的建議，以便從邊緣提供最佳化的體驗給AI代理程式。 如果CDN已完全路由，則網域中的所有頁面通常會在數分鐘內隨著新變更上線。 如果已針對選取的路徑設定路由，則只有列入允許清單的頁面會啟動最佳化。

![部署](/help/assets/optimize-at-edge/deploy.png)

### 即時檢視

**檢視即時**&#x200B;可讓您驗證最佳化是否即時，並如預期般執行一般流量，否則將難以存取此檢視。 您可以在「固定建議」下檢視即時頁面，這會將頁面呈現給AI代理程式。

![即時檢視](/help/assets/optimize-at-edge/view-live.png)

### 復原

復原可安全地回覆先前部署的最佳化。 僅限AI的頁面版本通常會在數分鐘內恢復其先前的狀態，以便您視需要安全地嘗試最佳化。

![回覆](/help/assets/optimize-at-edge/rollback.png)

## 常見問題

問：Edge的「最佳化」會鎖定哪些型別的LLM？

要鎖定的使用者代理清單由您在上線流程中定義。

<!--Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.-->

問：如果我還未加入Edge的最佳化，會發生什麼事？

如果您在完成必要的設定之前按一下&#x200B;**部署最佳化**，將不會有任何內容套用至您的網站。 反之，快顯對話方塊會提示您透過`llmo-at-edge@adobe.com`聯絡我們的團隊以尋求入門協助。 在上線完成之前，您仍然可以探索偵測到的機會和建議，但一鍵式部署工作流程將保持非使用中。

問：在來源更新內容時會發生什麼事？

只要基礎來源頁面未變更，我們就會從快取中提供您頁面的最佳化版本。 不過，當來源確實變更&#x200B;**復原內容可見度**&#x200B;時，我們的系統會自動重新整理，因此AI代理程式一律會收到最新的內容。 這是因為我們使用低快取存留時間(TTL)設定（依分鐘數順序），因此您網站上的任何內容更新都會觸發該視窗中的新最佳化。 針對&#x200B;**新增LLM友善摘要**等內容機會，LLM Optimizer會監控來源頁面是否有變更。 如果偵測到變更，我們會暫停最佳化並將其標幟為人類檢閱，以防止代理程式可見頁面和人類可見頁面之間的內容漂移。
<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

問：Edge最佳化是否僅適用於使用Adobe Edge Delivery Service (EDS)的網站？

不行。 在Edge中最佳化與CDN無關，且適用於任何前端架構，而不僅僅是Adobe的EDS棧疊上部署的架構。

問：Edge預先轉譯的「最佳化」與傳統伺服器端轉譯(SSR)有何不同？

兩者都可以解決不同的問題，並共同作業。 傳統SSR會呈現伺服器端內容，但不包含稍後在瀏覽器中載入的內容。 在Edge預先呈現時最佳化會在JavaScript和使用者端資料載入後擷取頁面，在CDN邊緣產生完整組合的版本。 SSR著重於改善人類體驗，而Edge最佳化則能改善LLM的網頁體驗。

問：如果我為網域中的部分URL （而非全部）部署最佳化，會發生什麼事？

只會修改您明確最佳化的URL。 對於具有已部署機會的URL，AI代理程式會收到最佳化版本。 對於沒有已部署機會的URL，我們的服務只會依原樣代理原始頁面，而不會套用變更或將其儲存在最佳化快取階層。 這可確保您可選擇性地部署最佳化，而不會影響網站的其餘部分。
