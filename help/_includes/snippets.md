---
source-git-commit: beae935e7a34f5bccbe21578fa9a928912958710
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 2%

---
# 程式碼片段

## 驗證設定 — Adobe Managed CDN {#verify-setup-adobe-aem-cs-cdn}

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

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

## 驗證設定 — BYOCDN {#verify-setup-byocdn}

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

## API金鑰擷取步驟 {#retrieve-byocdn-api-key}

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

## 返回總覽 {#return-to-overview}

若要進一步瞭解Edge最佳化，包括可用的機會、自動最佳化工作流程和常見問答，請返回[Edge最佳化概覽](/help/dashboards/optimize-at-edge.md)。
