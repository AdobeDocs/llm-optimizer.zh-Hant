---
title: 在Edge最佳化 — Apache HTTP Server
description: 瞭解如何在LLM Optimizer中設定Apache HTTP Server （自行託管反向Proxy） BYOCDN以在Edge進行最佳化。
feature: Opportunities
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
source-git-commit: d7e723161836027dcdde931378f5d0f776a1ecfc
workflow-type: tm+mt
source-wordcount: 585
ht-degree: 32%

---


# Apache HTTP 伺服器

當Apache HTTP Server在原始伺服器前面充當反向Proxy （自我託管設定，**不含** AEM Dispatcher）時，此設定即適用。 它將代理流量（來自AI機器人和LLM使用者代理程式的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中尋找 `x-edgeoptimize-request-id` 標頭。

整合是一組原生Apache `Include`檔案 — 沒有要部署的程式碼或背景工作。 您下載三個檔案、設定API金鑰，並將兩行`Include`新增至虛擬主機。

**先決條件**

設定Apache路由規則之前，請確定您已：

* 已啟用這些模組的Apache HTTP Server 2.4或更新版本： `proxy`、`proxy_http`、`ssl`、`rewrite`、`headers`、`env`和`setenvif`。
* 存取您的Apache設定（網站的`<VirtualHost>`）以及重新載入Apache的功能。
* 從 LLM Optimizer 使用者介面擷取的 Edge Optimize API 金鑰。 相關步驟請參閱[檢索 API 金鑰](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* (選用) 若要測試中繼路由，請參閱[中繼 API 金鑰](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

## 設定

**1. 下載組態檔**

從[Optimize at Edge程式碼範例存放庫](https://github.com/adobe/llmo-code-samples/tree/main/optimize-at-edge/apache)下載三個Edge Optimize包含檔案，並將它們放在Apache伺服器上的目錄中（例如`conf/oae/`）：

| 檔案 | 用途 |
|------|---------|
| `oae-routing.conf` | 偵測AI機器人、插入Edge Optimize標頭、將HTML頁面請求路由到後端，以及設定快取隔離和容錯移轉。 |
| `oae-failover.conf` | 如果Edge Optimize傳回錯誤，會根據您的來源重播原始請求。 |
| `domains.conf` | 啟用在每個網域的Edge最佳化，並保留您的API金鑰。 |

您不需要修改`oae-routing.conf`或`oae-failover.conf` — 按原樣使用。

**2. 啟用您的網域並設定API金鑰(`domains.conf`)**

編輯`domains.conf`並為您啟用的每個網域新增一行。 以您的網域取代主機，並使用LLM Optimizer UI中的金鑰取代`YOUR_API_KEY`。 未列出的網域路由到原始未變更，因此您可以一次啟用一個網域。

```
SetEnvIfExpr "%{HTTP_HOST} =~ m#(?i)^(www\.)?example\.com(:\d+)?$#" OAE_DOMAIN_ENABLED=1 OAE_API_KEY=YOUR_API_KEY
```

**3. 將檔案包含在虛擬主機中**

將兩個`Include`行新增到您現有的`<VirtualHost *:443>`。 路由檔案&#x200B;**在**&#x200B;您的重寫和`ProxyPass`規則之前；容錯移轉檔案在&#x200B;**之後**&#x200B;進行。 在下列範例中，標示為`#NEWLINE`的行是您在Edge為「最佳化」新增的唯一行 — 其他所有行（`ServerName`、`ProxyPass`及其他）都是您現有的未變更設定。

```
Define OAE_CONF_DIR conf/oae                       #NEWLINE  directory holding the OAE include files

<VirtualHost *:443>
    ServerName www.example.com

    Include "${OAE_CONF_DIR}/oae-routing.conf"     #NEWLINE  OAE routing — BEFORE your Rewrite & ProxyPass rules

    # --- your existing rewrite rules and ProxyPass to origin ---
    ProxyPass        "/" "https://www.example.com/"
    ProxyPassReverse "/" "https://www.example.com/"

    Include "${OAE_CONF_DIR}/oae-failover.conf"    #NEWLINE  OAE failover — AFTER your ProxyPass rules
</VirtualHost>
```

**4. 重新載入Apache**

驗證設定並重新載入Apache以套用變更。

>[!NOTE]
>
>機器人最佳化和人工回應會自動保留在個別的快取專案中（路由檔案集`Vary: x-edgeoptimize-config`）。 如果您的Apache已使用`mod_cache`，請確定它有`CacheQuickHandler Off`，以便在Edge Optimize標頭設定完成後執行快取查閱。

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
| `x-edgeoptimize-fo` | 唯有發生容錯移轉時存在 (值：`1`) | 不存在 |

{{verify-routing-status-in-ui}}

{{return-to-overview}}
