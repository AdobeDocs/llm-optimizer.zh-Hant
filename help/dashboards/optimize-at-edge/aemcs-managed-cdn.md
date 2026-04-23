---
title: 邊緣最佳化：AEM Cloud Service 管理的內容傳遞網路 (Fastly)
description: 了解在 LLM Optimizer 中如何設定 AEM Cloud Service 管理的內容傳遞網路 (Fastly) 進行邊緣最佳化。
feature: Opportunities
source-git-commit: 184d6008c2579014c6ff453e8bfff4bb898f4b82
workflow-type: ht
source-wordcount: '836'
ht-degree: 100%

---


# AEM Cloud Service 管理的內容傳遞網路 (Fastly)

此設定會將代理式流量 (來自 AI 機器人和 LLM 使用者代理的要求) 路由至 Edge Optimize 後端服務 (`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中檢查 `x-edgeoptimize-request-id` 標頭。

## 先決條件

若要存取此功能：

- 付費客戶必須擁有 **Adobe LLM Optimizer 使用者** IMS 產品設定檔的存取權。 請聯絡貴組織的管理員以要求存取權。
  ![將使用者新增至產品設定檔](/help/assets/optimize-at-edge/cs-fastly-user-product-profiles.png)
- 試用版客戶必須屬於 **LLMO 管理員** IMS 群組。 如果群組不存在，則組織的管理員可以建立群組並把您新增到群組中。
  ![建立 LLMO 管理 IMS 群組](/help/assets/optimize-at-edge/cs-fastly-create-ims-group.png)

>[!NOTE]
> Safari 或無痕/私人瀏覽模式不支援此功能。

## 啟用路由的步驟

若要開始將代理流量路由至 Edge Optimize：

1. 在 LLM Optimizer 中，開啟「**客戶設定**」並選取「**內容傳遞網路設定**」標籤。

   ![導覽至客戶設定](/help/assets/optimize-at-edge/cs-fastly-prereq-customer-config-nav.png)

2. 找出「**將最佳化部署到 AI 代理**」區段。 按一下&#x200B;**「啟用」**&#x200B;按鈕。

   ![將最佳化部署到 AI 代理：待處理](/help/assets/optimize-at-edge/cs-fastly-enable-button.png)

3. 在確認對話框中，選取&#x200B;**「啟用」**&#x200B;以確認您要啟用路由。 如果出現錯誤，請參閱[疑難排解](#troubleshooting)區段以解決問題。

   ![啟用最佳化引擎確認對話框](/help/assets/optimize-at-edge/cs-fastly-enable-dialog.png)

4. 確認後，需要幾分鐘才能完成路由。

   ![路由進行中](/help/assets/optimize-at-edge/cs-fastly-enable-button-clicked-routing-in-progress.png)

   5 分鐘後重新載入頁面，以確認路由已完成。 路由設定好並啟動後，狀態會更新為「**已完成**」，並顯示綠色勾號確認路由已啟用。 您無需再執行任何動作。

   ![將最佳化部署到 AI 代理：已完成](/help/assets/optimize-at-edge/cs-fastly-disable-button.png)

   若要隨時停用路由，請返回&#x200B;**「內容傳遞網路設定」**&#x200B;分頁標籤中的&#x200B;**「將最佳化部署到 AI 代理」**&#x200B;區段，然後按一下&#x200B;**「停用」**。

此外，如果您對於上述步驟需要任何協助，請聯絡您的 Adobe 帳戶團隊或 `llmo-at-edge@adobe.com`。

## 疑難排解

如果在啟用或停用路由時發生錯誤，顯示內容將與以下範例類似：

![確認對話框錯誤](/help/assets/optimize-at-edge/cs-fastly-confirmation-dialog-error.png)

使用以下清單來識別錯誤及遵循指示。

1. **使用者沒有 LLMO 產品存取權**

   **原因：**&#x200B;在您的 Adobe IMS 設定檔中使用者帳戶不具備 LLM Optimizer 產品內容。 付費客戶必須具備該內容才能設定內容傳遞網路路由。

   **建議：**&#x200B;確認您的組織管理員已在 Adobe Admin Console 中為您指派 **Adobe LLM Optimizer 使用者**&#x200B;產品設定檔。

2. **只有 LLMO 管理員群組成員可以設定內容傳遞網路路由**

   **原因：**&#x200B;您的帳戶不是 **LLMO 管理員** IMS 群組的成員。 試用版客戶必須是此群組成員才能設定內容傳遞網路路由。

   **建議：**&#x200B;確認您的組織管理員已將您新增至 Adobe Admin Console 中的 **LLMO 管理員** IMS 群組。

3. **要求的內容傳遞網路類型 aem-cs-fastly 不符合這個網域偵測到的內容傳遞網路**

   **原因：**&#x200B;這表示偵測到您網站的內容傳遞網路類型不是 *AEM Cloud Service 管理的內容傳遞網路 (Fastly)*。

   **建議：**&#x200B;確認您的網站是透過 AEM Cloud Service 管理的內容傳遞網路 (Fastly) 提供。

4. **探測網站時發生錯誤**

   **原因：** LLM Optimizer 在路由設定期間無法連線到您的網站。 如果網站關閉、無法連線或要求逾時，便可能發生此狀況。

   **建議：**&#x200B;確認您的網站可公開存取及傳回有效的回應，然後再試一次。

5. **對於路由探測網站並未傳回有效回應**

   **原因：**&#x200B;在設定期間進行探測時，網站傳回非預期的 HTTP 狀態 (不是 2xx 或 301)。

   **建議：**&#x200B;確認您的網站針對 LLM Optimizer 中註冊的基礎 URL 傳回成功的回應 (2xx)，然後再試一次。

6. **上游 IMS 服務的驗證失敗**

   **原因：**&#x200B;工作階段可能已過期，或在路由要求期間 Adobe IMS 驗證發生問題。

   **建議：**&#x200B;登出 LLM Optimizer 並重新登入，然後再次嘗試啟用路由。

如果問題仍然存在，請聯絡您的 Adobe 帳戶團隊或 `llmo-at-edge@adobe.com`。

## (選用) 驗證設定

路由設定完成後，您可以選擇是否驗證 AI 機器人流量已路由至 Edge Optimize，以及真人流量不受影響。

1. **測試機器人流量 (應經過最佳化)**

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

2. **測試真人流量 (不應受到影響)**

   模擬一般真人瀏覽器要求：

   ```
   curl -svo /dev/null https://www.example.com/page.html \
     --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
   ```

   回應不應包含 `x-edgeoptimize-request-id` 標頭。 頁面內容和回應時間應與啟用邊緣最佳化之前維持相同。

3. **如何區分這兩種情境**

   | 頁首 | 機器人流量 (最佳化) | 真人流量 (不受影響) |
   |---|---|---|
   | `x-edgeoptimize-request-id` | 存在：包含唯一的要求 ID | 不存在 |
   | `x-edgeoptimize-fo` | 唯有發生容錯移轉時存在 (值：`1`) | 不存在 |

4. **檢查 LLM Optimizer 中的路由狀態**

   您也可以在 LLM Optimizer UI 中確認路由。 開啟「**客戶設定**」並選取「**內容傳遞網路設定**」標籤。 當路由已啟用時，「**將最佳化部署到 AI 代理**」區段會顯示「**已完成**」。

   ![將最佳化部署到 AI 代理：已完成](/help/assets/optimize-at-edge/cs-fastly-disable-button.png)

{{return-to-overview}}
