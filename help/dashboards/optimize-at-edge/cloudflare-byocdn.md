---
title: 在Edge最佳化 — Cloudflare (BYOCDN)
description: 瞭解如何在LLM Optimizer中為Edge最佳化設定Cloudflare BYOCDN。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '1402'
ht-degree: 1%

---


# Cloudflare (BYOCDN)

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

在設定Cloudflare Worker路由規則之前，請確定您具有：

* 在您的網域中啟用Worker的Cloudflare帳戶。
* 在Cloudflare中存取您網域的DNS設定。
* 已完成LLM Optimizer上線程式。
* 已完成CDN記錄檔轉送至LLM Optimizer。
* 從Edge UI擷取的LLM Optimizer最佳化API金鑰。

{{retrieve-byocdn-api-key}}

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
