---
title: 邊緣最佳化：Fastly (BYOCDN)
description: 了解在 LLM Optimizer 中如何設定 Fastly BYOCDN 進行邊緣最佳化。
feature: Opportunities
source-git-commit: 13d2f4bbd1f9d3886f89f80df0e76093f2afdf13
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 93%

---


# Fastly (BYOCDN)

此設定會將代理式流量 (來自 AI 機器人和 LLM 使用者代理的要求) 路由至 Edge Optimize 後端服務 (`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中尋找 `x-edgeoptimize-request-id` 標頭。

**先決條件**

設定 Fastly VCL 規則之前，請確定您具備以下條件：

* 您網域的 Fastly 存取權。
* 從 LLM Optimizer 使用者介面擷取的 Edge Optimize API 金鑰。 如需步驟，請參閱[擷取您的API金鑰](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* （選擇性）若要測試暫存路由，請參閱[暫存API金鑰](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

**設定**

將下列三個 VCL 程式碼片段新增至您的 Fastly 服務。 這些程式碼片段會處理對 Edge Optimize、快取金鑰分離和容錯移轉至預設來源的路由代理式要求。

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![新增 VCL 程式碼片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv 程式碼片段**

```
unset req.http.x-edgeoptimize-url;
unset req.http.x-edgeoptimize-config;
unset req.http.x-edgeoptimize-api-key;
unset req.http.x-edgeoptimize-fetcher-key; # Optional (required only in case of WAF)

if (!req.http.x-edgeoptimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-forwarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edgeoptimize-url = req.url; # required for identifying the original url
  set req.http.x-edgeoptimize-config = "LLMCLIENT=TRUE;"; # required for cache key
  set req.http.x-edgeoptimize-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.http.x-edgeoptimize-fetcher-key = "<YOUR FETCHER KEY>"; # Optional (required only in case of WAF)
  set req.backend = F_EDGE_OPTIMIZE;
}
```

**vcl_hash 程式碼片段**

```
if (req.http.x-edgeoptimize-config) {
  set req.hash += "edge-optimize";
  set req.hash += req.http.x-edgeoptimize-config;
}
```

**vcl_deliver 程式碼片段**

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

`vcl_deliver` 程式碼片段會自動處理容錯移轉。 如果 Edge Optimize 傳回 `4XX` 或 `5XX` 錯誤，該要求會重新啟動並路由回到您的預設來源，讓一般使用者仍能收到回應。 容錯移轉回應包含 `x-edgeoptimize-fo: 1` 標頭。

| 情境 | 行為 |
| --- | --- |
| Edge Optimize 傳回 `2XX` | 最佳化的回應會傳送至用戶端。 |
| Edge Optimize 傳回 `4XX` 或 `5XX` | 系統會重新啟動要求，並從預設來源提供。 |
| 容錯移轉回應 | 包含標頭 `x-edgeoptimize-fo: 1`。 |

**允許透過防火牆規則在Edge最佳化（選用）**

{{waf-allowlist-setup}}

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

{{return-to-overview}}
