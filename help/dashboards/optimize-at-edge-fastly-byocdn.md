---
title: 在Edge最佳化 — Fastly (BYOCDN)
description: 瞭解如何在LLM Optimizer中為Edge最佳化設定Fastly BYOCDN。
feature: Opportunities
source-git-commit: 8cdd15413555057165f69ea4d5a15b243ab9098d
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 6%

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

{{verify-setup-byocdn}}

{{return-to-overview}}
