---
title: 邊緣最佳化：Cloudflare (BYOCDN)
description: 了解在 LLM Optimizer 中如何設定 Cloudflare BYOCDN 進行邊緣最佳化。
feature: Opportunities
source-git-commit: 14dbee36f39b0d993d448edccb63fb8a519704a1
workflow-type: tm+mt
source-wordcount: '1922'
ht-degree: 69%

---


# Cloudflare (BYOCDN)

此設定會將代理式流量 (來自 AI 機器人和 LLM 使用者代理的要求) 路由至 Edge Optimize 後端服務 (`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中尋找 `x-edgeoptimize-request-id` 標頭。

**先決條件**

設定 Cloudflare Worker 路由規則之前，請確定您具備下列條件：

* 於網域上具備已啟用 Worker 功能的 Cloudflare 帳戶。
* 具備在 Cloudflare 中存取您網域 DNS 設定的存取權。
* 已完成 LLM Optimizer 上線流程。
* 已經將內容傳遞網路記錄轉送至 LLM Optimizer。
* 從 LLM Optimizer 使用者介面擷取的 Edge Optimize API 金鑰。
* （選用）如果先在中繼主機名稱上測試路由，則在中繼Edge最佳化API金鑰。

{{retrieve-byocdn-api-key}}

{{retrieve-staging-edge-optimize-api-key}}

**路由如何運作**

若設定正確，Cloudflare Worker 會攔截代理式使用者代理對您網域 (例如 `www.example.com/page.html`) 發出的要求，並將要求路由至 Edge Optimize 後端。 後端要求包含必要的標頭。

**測試後端要求**

您可以直接向 Edge Optimize 後端提出要求，藉以驗證路由。

```
curl -svo /dev/null https://live.edgeoptimize.net/page.html \
  -H 'x-forwarded-host: www.example.com' \
  -H 'x-edgeoptimize-url: /page.html' \
  -H 'x-edgeoptimize-api-key: $EDGE_OPTIMIZE_API_KEY' \
  -H 'x-edgeoptimize-config: LLMCLIENT=TRUE;'
```

**必要的標頭**

對 Edge Optimize 後端的要求必須設定以下標頭：

| 頁首 | 說明 | 範例 |
|--------|-------------|---------|
| `x-forwarded-host` | 要求的原始主機。 識別網站網域時需要使用。 | `www.example.com` |
| `x-edgeoptimize-url` | 要求的原始 URL 路徑和查詢字串。 | `/page.html` 或 `/products?id=123` |
| `x-edgeoptimize-api-key` | Adobe 為您網域提供的 API 金鑰。 | `your-api-key-here` |
| `x-edgeoptimize-config` | 快取鍵差異化的設定字串。 | `LLMCLIENT=TRUE;` |

## 設定選項

有兩種方法可設定Cloudflare Worker for Edge最佳化：

* [**選項1：部署至Cloudflare （建議）**](#option-1-deploy-to-cloudflare) — 自動建立新的背景工作，並提示您輸入必要的環境變數和密碼。 如果您沒有此網域的現有Cloudflare Worker，請使用此選項。
* [**選項2：手動設定**](#option-2-manual-setup) — 自行建立和設定背景工作程式的逐步指示。 如果您已有要擴充的現有Cloudflare Worker，或您想要完全控制部署，請使用此選項。

無論您選擇哪個選項，都必須手動將背景工作者連結至您的網域 — 請參閱[步驟：新增路由至您的網域](#add-a-route-to-your-domain)。

## 選項1：部署至Cloudflare

此選項使用&#x200B;**部署到Cloudflare**&#x200B;按鈕來自動建立背景工作，並在您的Cloudflare帳戶中設定必要的環境變數和密碼。 如果您正在設定新的背景工作，這是最快速入門的方法。

>[!IMPORTANT]
>
>只有在您&#x200B;**沒有**&#x200B;您的網域中已有Cloudflare Worker時，才使用此選項。 如果您已有背景工作，請使用[選項2：手動設定](#option-2-manual-setup)，將Edge最佳化路由邏輯新增至現有的背景工作。

**步驟1：部署背景工作**

按一下下方的按鈕，將Edge最佳化背景工作部署至您的Cloudflare帳戶：

[![部署至Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/adobe/llmo-code-samples/tree/main/optimize-at-edge/cloudflare/automation)

**步驟2：填寫部署表單**

按一下按鈕可開啟Worker設定頁面。 請依照以下說明填寫表單：

![Cloudflare Worker設定頁面](/help/assets/optimize-at-edge/cloudflare-deploy-form.png)

1. **Git帳戶** — 從下拉式清單中選取您的GitHub或GitLab帳戶。 Cloudflare會將背景工作程式碼分叉至您帳戶的存放庫。 如果未列出任何帳戶，您可以直接從下拉式清單中選取&#x200B;**+新GitHub連線**&#x200B;或&#x200B;**+新GitLab連線**，以新增連線。 如需詳細資訊，請參閱[Cloudflare Git整合指南](https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/github-integration/)。

   ![Git帳戶下拉式清單，顯示新的GitHub連線和新的GitLab連線選項](/help/assets/optimize-at-edge/cloudflare-git-connection.png)
2. **建立私人Git存放庫** — 保留此為勾選狀態（預設）。
3. **專案名稱** — 請保留`edge-optimize-router`或輸入您選擇的名稱。
4. **EDGE_OPTIMIZE_API_KEY** — 貼上Adobe提供的Edge最佳化API金鑰。 此值會儲存為加密的密碼。
5. **EDGE_OPTIMIZE_TARGET_HOST** — 輸入您網站不含通訊協定的網域（例如，`www.example.com`）。
6. **組建命令** — 留空。
7. **部署命令** — 保留為`npm run deploy` （預填）。
8. **非生產分支的組建** — 保持未勾選。 這是開發人員工作流程功能，不是此部署所需的功能。
9. 按一下&#x200B;**建立並部署**。

在部署背景工作之後，請繼續到[新增您網域的路由](#add-a-route-to-your-domain)以將背景工作與您的網域連結。 路由未自動設定，必須手動完成。

## 選項2：手動設定

請依照下列步驟手動建立和設定背景工作。

**步驟 1：建立 Cloudflare Worker**

1. 登入您的 Cloudflare 儀表板。
2. 在側邊欄中導覽至「**Worker 與頁面**」。
3. 按一下「**建立應用程式**」，然後按一下「**建立 Worker**」。
4. 為您的 Worker 命名 (例如 `edge-optimize-router`)。
5. 按一下「**部署**」，使用預設程式碼建立 Worker。

![Cloudflare Worker 儀表板](/help/assets/optimize-at-edge/cloudflare-workers-dashboard.png)

**步驟 2：新增 Worker 程式碼**

建立 Worker 之後，按一下「**編輯程式碼**」，然後用下列內容取代預設程式碼：

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

按一下「**儲存並部署**」以發佈 Worker。

![Cloudflare Worker 程式碼編輯器](/help/assets/optimize-at-edge/cloudflare-worker-editor.png)

**步驟3：設定環境變數和秘密**

環境變數會安全地儲存敏感設定，例如您的 API 金鑰。

1. 在您的 Worker 設定中，導覽至「**設定**」>「**變數**」。
2. 在「**環境變數**」之下，按一下「**新增變數**」。
3. 新增下列變數：

   | 變數名稱 | 說明 | 必要 |
   |---------------|-------------|----------|
   | `EDGE_OPTIMIZE_API_KEY` | Adobe 提供的 Edge Optimize API 金鑰。 | 是 |
   | `EDGE_OPTIMIZE_TARGET_HOST` | Edge Optimize 要求的目標主機 (以 `x-forwarded-host` 標頭傳送) 以及容錯移轉的原始網域。 必須是沒有通訊協定的網域 (例如 `www.example.com`，而非 `https://www.example.com`)。 | 是 |

4. 對於 API 金鑰，按一下「**加密**」，安全地將其儲存。
5. 按一下「**儲存並部署**」。

![Cloudflare 環境變數](/help/assets/optimize-at-edge/cloudflare-env-variables.png)

## 新增路由至您的網域 {#add-a-route-to-your-domain}

無論您使用哪個設定選項，都必須手動將背景工作程式連結至您的網域。 此步驟會針對您的流量啟動背景工作。

1. 前往 Worker 的「**設定**」>「**觸發程序**」。
2. 在「**路由**」之下，按一下「**新增路由**」。
3. 輸入您的網域模式 (例如 `www.example.com/*` 或 `example.com/*`)。
4. 從下拉式清單中選取您的區域。
5. 按一下&#x200B;**儲存**。

或者，您可以在區域層級設定路由：

1. 在 Cloudflare 中導覽至您的網域。
2. 前往「**Worker 路由**」。
3. 按一下「**新增路由**」並指定模式和 Worker。

![Cloudflare Worker 路由](/help/assets/optimize-at-edge/cloudflare-worker-routes.png)

**驗證容錯移轉行為**

如果 Edge Optimize 無法使用或傳回錯誤，Worker 會自動容錯移轉至您的來源。 容錯移轉回應包含 `x-edgeoptimize-fo` 標頭：

```
< HTTP/2 200
< x-edgeoptimize-fo: 1
```

您可以在 Cloudflare Worker 記錄中監視容錯移轉事件，以便進行疑難排解。

**了解 Worker 邏輯**

Cloudflare Worker 會實施下列邏輯：

1. **使用者代理偵測：**&#x200B;檢查傳入要求的使用者代理是否與任何已定義的代理式機器人相符 (不區分大小寫)。

2. **路徑目標選擇：**&#x200B;根據目標路徑選擇性篩選要求。 預設情況下，會路由所有 HTML 頁面 (以 `/`、無副檔名或 `.html` 結尾的 URL)。 您可以使用 `TARGETED_PATHS` 陣列指定特定路徑。

3. **迴圈保護：**`x-edgeoptimize-request` 標頭能防止無限迴圈。 當 Edge Optimize 將要求傳回您的來源時，此標頭設為 `"1"`，而 Worker 傳遞要求時不會將其路由回到 Edge Optimize。

4. **標頭安全性：**&#x200B;在設定 Edge Optimize 標頭之前，Worker 會移除傳入要求中任何現有的 `x-edgeoptimize-*` 標頭，以避免標頭注入攻擊。

5. **標頭對應：** Worker 會設定 Edge Optimize 的必要標頭：
   * `x-forwarded-host`：識別原始網站網域。
   * `x-edgeoptimize-url`：保留原始要求路徑和查詢字串。
   * `x-edgeoptimize-api-key`：使用 Edge Optimize 驗證要求。
   * `x-edgeoptimize-config`：提供快取鍵設定。

6. **容錯移轉邏輯：**&#x200B;如果 Edge Optimize 傳回任何錯誤狀態代碼 (4XX 用戶端錯誤或 5XX 伺服器錯誤)，或要求因網路錯誤而失敗，Worker 會使用 `EDGE_OPTIMIZE_TARGET_HOST` 自動容錯移轉至您的來源。 容錯移轉回應包含 `x-edgeoptimize-fo: 1` 標頭，用於表示已發生容錯移轉。

7. **重新導向處理：**`redirect: "manual"` 選項可以確保來自 Edge Optimize 的重新導向回應會傳遞至用戶端，而 Worker 不會追隨重新導向。

**自訂設定**

您可以修改程式碼頂端的設定常數，自訂 Worker 的行為：

**代理式機器人清單**

修改 `AGENTIC_BOTS` 陣列以新增或移除使用者代理：

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

預設情況下，所有 HTML 頁面都會路由至 Edge Optimize。 若要將路由限制在特定路徑，請修改 `TARGETED_PATHS` 陣列：

```javascript
// Route all HTML pages (default)
const TARGETED_PATHS = null;

// Or specify exact paths to route
const TARGETED_PATHS = ['/', '/page.html', '/products', '/about-us'];
```

**容錯移轉設定**

預設情況下，Worker 會在 Edge Optimize 發生任何 4XX 或 5XX 錯誤時進行容錯移轉。 自訂此行為：

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

* **容錯移轉行為：**&#x200B;如果 Edge Optimize 傳回任何錯誤 (4XX 或 5XX 狀態代碼)，或要求因網路錯誤而失敗，Worker 會自動容錯移轉至您的來源。 容錯移轉使用 `EDGE_OPTIMIZE_TARGET_HOST` 做為原始網域 (類似 Fastly 的 `F_Default_Origin` 或 CloudFront 的 `Default_Origin`)。 容錯移轉回應包含 `x-edgeoptimize-fo: 1` 標頭，可用於監視和偵錯。

* **快取：** Cloudflare 預設會根據 URL 快取回應。 由於代理式流量接收的內容與真人流量不同，請確保您的快取設定將此情況納入考量。 請考慮使用快取 API 或快取標頭區分快取的內容。 您的快取鍵中應包含 `x-edgeoptimize-config` 標頭。

* **速率限制：**&#x200B;監視您的 Edge Optimize 使用情形，並視需要考慮對代理式流量實施速率限制。

* **測試：**&#x200B;部署至生產環境之前，請一律先在中繼環境中測試設定。 確認代理式和真人流量的運作皆符合預期。 透過模擬 Edge Optimize 錯誤來測試容錯移轉行為。

* **記錄：**&#x200B;啟用 Cloudflare Worker 記錄，以便監視要求及進行疑難排解。 導覽至「**Workers**」>「**您的 Worker**」>「**記錄**」來檢視即時記錄。 Worker 記錄容錯移轉事件以供偵錯使用。

**疑難排解**

| 問題 | 可能的原因 | 解決方案 |
|-------|----------------|----------|
| 回應中沒有 `x-edgeoptimize-request-id` 標頭 | Worker 路由不相符，或使用者代理不在代理式機器人清單中。 | 確認您的路由模式符合要求 URL。 檢查使用者代理是否在 `AGENTIC_BOTS` 陣列中。 |
| Edge Optimize 中的 401 或 403 錯誤 | API 金鑰無效或遺失。 | 驗證已在環境變數和密碼中正確設定`EDGE_OPTIMIZE_API_KEY`。 聯絡 Adobe 確認您的 API 金鑰有效。 |
| 無限重新導向或迴圈 | 未正確設定或檢查迴圈保護標頭。 | 確保 `x-edgeoptimize-request` 標頭檢查已就緒。 |
| 真人流量受到影響 | Worker 路由邏輯過於廣泛。 | 確認使用者代理的對應邏輯正確且不區分大小寫。 檢查 `TARGETED_PATHS` 的設定正確。 |
| 回應速度緩慢 | Edge Optimize 後端的網路延遲。 | 已預期第一個要求會發生這樣的情況；後續要求會快取至 Edge Optimize。 |
| 回應中的 `x-edgeoptimize-fo: 1` 標頭 | Edge Optimize 傳回錯誤，並容錯移轉至來源。 | 檢查 Cloudflare Worker 記錄中的特定錯誤代碼。 向 Adobe 確認 Edge Optimize 的服務狀態。 |
| 容錯移轉未正常運作 | 容錯移轉標記已停用，或容錯移轉邏輯發生錯誤。 | 確認 `FAILOVER_ON_4XX` 和 `FAILOVER_ON_5XX` 是否設定為 `true`。 檢查 Worker 記錄中的錯誤訊息。 |
| 某些路徑未最佳化 | 路徑不符合目標路徑或 HTML 頁面模式。 | 確認路徑是否在 `TARGETED_PATHS` (若已指定) 中，並且符合 HTML 頁面規則運算式模式。 |
| 要求因為主機無效而執行失敗 | `EDGE_OPTIMIZE_TARGET_HOST` 包含通訊協定 (例如 `https://`)。 | 僅能使用沒有通訊協定的網域名稱 (例如 `example.com`，而非 `https://example.com`)。 |
| 容錯移轉期間發生 530 錯誤 | Cloudflare 無法連線至來源，或容錯移轉要求含有無效的標頭。 | 確保容錯移轉功能會移除 Edge Optimize 標頭。 確認您的來源可供存取，且 DNS 的設定正確。 |

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

**4. 暫存網域（選擇性）**

如果您使用來自LLM Optimizer的暫存主機名稱與暫存API金鑰，請使用&#x200B;**暫存** API金鑰在您的&#x200B;**暫存**&#x200B;區域上部署相同的Worker邏輯。 接著，驗證中繼主機上的機器人流量：

```
curl -svo /dev/null https://staging.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

將`https://staging.example.com/page.html`取代為您的實際暫存URL和路徑。 成功的回應包含`x-edgeoptimize-request-id`標頭。

{{verify-routing-status-in-ui}}

{{return-to-overview}}
