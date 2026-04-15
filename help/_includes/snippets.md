---
source-git-commit: e9309dc8f8d1d81b953483f17dcb424e46d5cd3b
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 20%

---
# 程式碼片段

## API金鑰擷取步驟 {#retrieve-byocdn-api-key}

**擷取生產Edge最佳化API金鑰的步驟：**

1. 在 LLM Optimizer 中，開啟「**客戶設定**」並選取「**內容傳遞網路設定**」標籤。

   ![導覽至客戶設定](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找出「**將最佳化部署到 AI 代理**」區段。 勾選「**啟用最佳化引擎**」核取方塊。

   ![將最佳化部署到 AI 代理：待處理](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在確認對話框中，選取「**啟用**」。

   ![啟用最佳化引擎確認對話框](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. 選取&#x200B;**檢視詳細資料**。 在&#x200B;**部署最佳化詳細資料**&#x200B;對話方塊中，複製&#x200B;**生產API金鑰** （使用欄位旁的&#x200B;**複製**）。

   部署最佳化詳細資料中的![生產API金鑰](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >對話方塊可能會顯示設定未完成。 在驗證路由之前，這是預期中的情形 — 您仍可複製API金鑰，讓您的IT或CDN團隊完成設定。

此外，如果您對於上述步驟需要任何協助，請聯絡您的 Adobe 帳戶團隊或 `llmo-at-edge@adobe.com`。

## 可選：測試測試主機名稱上的路由 {#retrieve-staging-edge-optimize-api-key}

**選用：測試暫存主機名稱上的路由**

若您想在啟用生產製程之前，先在較低的環境中驗證製程，則可設定暫存主機名稱。

**需求**

* 暫存主機名稱必須位於與生產環境相同的&#x200B;**可登入網域**&#x200B;上（例如，當生產環境為`https://www.example.com`時，`https://staging.example.com`）。
* 每個網站只有&#x200B;**一個**&#x200B;暫存網域。 儲存後，必須聯絡Adobe才能變更。

**取得您的暫存API金鑰**

1. 開啟&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**。
2. 在&#x200B;**將最佳化部署到AI代理程式**&#x200B;下，選取&#x200B;**新增中繼網域** （或如果已設定中繼網域，則選取&#x200B;**中繼網域**）。
3. 輸入包含`https://`的完整暫存URL並選取&#x200B;**設定網域**。
4. 從確認對話方塊複製&#x200B;**staging** API金鑰。

![中繼網域API金鑰](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

使用中繼API金鑰在中繼環境中部署相同的路由規則。

**測試暫存機器人流量**

請使用您實際的中繼 URL 和路徑取代 `https://staging.example.com/page.html`。 **成功：**&#x200B;回應包含`x-edgeoptimize-request-id`標頭。

如果您需要協助，請連絡`llmo-at-edge@adobe.com`。

## 檢查LLM Optimizer中的路由狀態 {#verify-routing-status-in-ui}

您也可以在 LLM Optimizer 使用者介面中確認流量路由的狀態。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

![將最佳化部署到 AI 代理：已完成](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

## 允許透過防火牆規則在Edge最佳化（選用） {#waf-allowlist-setup}

如果您的CDN使用WAF或機器人管理員：

* 允許列出WAF或機器人管理員中的`*AdobeEdgeOptimize/1.0*`使用者代理程式，讓Edge最佳化服務可以擷取您的來源內容。
* 如果您的防火牆需要使用者代理程式以外的其他驗證，請產生密碼（例如，`openssl rand -hex 32`）並：
   * 將帶有密碼的`x-edgeoptimize-fetcher-key`新增到路由規則中與其他`x-edgeoptimize-*`標頭一起。
   * 新增WAF或機器人管理員規則以允許`x-edgeoptimize-fetcher-key`符合相同密碼的請求。
* 在Edge最佳化會依原樣轉送此標題 — 您擁有完整的金鑰生命週期。

## 返回總覽 {#return-to-overview}

若要進一步瞭解Edge最佳化，包括可用的機會、自動最佳化工作流程和常見問答，請返回[Edge最佳化概覽](/help/dashboards/optimize-at-edge/overview.md)。
