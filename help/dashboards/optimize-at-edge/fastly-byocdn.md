---
title: 在Edge最佳化 — Fastly (BYOCDN)
description: 瞭解如何在LLM Optimizer中為Edge最佳化設定Fastly BYOCDN。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 5%

---


# Fastly (BYOCDN)

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

在設定Fastly VCL規則之前，請確定您具有：

* 為您的網域存取Fastly。
* 已完成LLM Optimizer上線程式。
* 已完成CDN記錄檔轉送至LLM Optimizer。
* 從Edge UI擷取的LLM Optimizer最佳化API金鑰。

{{retrieve-byocdn-api-key}}

**組態**

將下列三個VCL片段新增至您的Fastly服務。 這些程式碼片段會處理路由代理對Edge Optimize、快取金鑰分離和容錯移轉至預設來源的請求。

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![新增 VCL 程式碼片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv 程式碼片段**

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

`vcl_deliver`程式碼片段會自動處理容錯移轉。 如果Edge Optimize傳回`4XX`或`5XX`錯誤，要求會重新啟動並路由回您的預設來源，讓一般使用者仍會收到回應。 容錯移轉回應包含`x-edgeoptimize-fo: 1`標頭。

| 情境 | 行為 |
| --- | --- |
| Edge最佳化傳回`2XX` | 最佳化的回應會提供給使用者端。 |
| Edge最佳化傳回`4XX`或`5XX` | 系統會重新啟動要求，並從預設來源提供服務。 |
| 容錯移轉回應 | 包含標頭`x-edgeoptimize-fo: 1`。 |

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
