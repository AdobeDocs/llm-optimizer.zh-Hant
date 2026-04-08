---
source-git-commit: da789100d814004687de2f46e18a295671dec4b8
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 10%

---
# 程式碼片段

## API金鑰擷取步驟 {#retrieve-byocdn-api-key}

**擷取生產Edge最佳化API金鑰的步驟：**

1. 在LLM Optimizer中，開啟&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

   ![導覽至客戶設定](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找到&#x200B;**將最佳化部署到AI代理程式**&#x200B;區段。 勾選&#x200B;**啟用最佳化引擎**&#x200B;核取方塊。

   ![將最佳化部署到AI代理程式 — 擱置中](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在確認對話方塊中，選取&#x200B;**啟用**。

   ![啟用最佳化引擎確認對話方塊](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. 選取&#x200B;**檢視詳細資料**。 在&#x200B;**部署最佳化詳細資料**&#x200B;對話方塊中，複製&#x200B;**生產API金鑰** （使用欄位旁的&#x200B;**複製**）。

   部署最佳化詳細資料中的![生產API金鑰](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >對話方塊可能會顯示設定未完成。 在驗證路由之前，這是預期中的情形 — 您仍可複製API金鑰，讓您的IT或CDN團隊完成設定。

此外，如果您對於上述步驟需要任何協助，請聯絡您的 Adobe 帳戶團隊或 `llmo-at-edge@adobe.com`。

## 中繼網域API金鑰（選擇性） {#retrieve-staging-edge-optimize-api-key}

如果要在生產流量使用路由規則之前，於較低層級環境中測試Edge的「最佳化」，請使用預備主機名稱。

**先決條件**

* 暫存主機名稱必須屬於與您的生產網站相同的&#x200B;**可登入網域** （例如，當生產網站為`https://www.example.com`時，`https://staging.example.com`）。
* 只能為網站設定&#x200B;**一個**&#x200B;暫存網域。 儲存後，必須協助才能變更。

**步驟**

1. 在LLM Optimizer中，開啟&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

2. 在&#x200B;**將最佳化部署到AI代理程式**&#x200B;區段中，選取&#x200B;**新增中繼網域** (或者&#x200B;**中繼網域** （如果已設定中繼網域）。

3. 在&#x200B;**中繼網域**&#x200B;對話方塊中，輸入包含`https://`的完整暫存URL，並選取&#x200B;**設定網域**。

   ![中繼網域輸入對話方塊](/help/assets/optimize-at-edge/byocdn-staging-domain-input.png)

4. 在下一個提示中確認網域。 工作流程完成時，**階段網域**&#x200B;對話方塊會顯示已設定的網域及其&#x200B;**API金鑰**。 選取&#x200B;**複製**&#x200B;以複製暫存API金鑰。

   ![中繼網域API金鑰](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

如果您需要協助，請連絡`llmo-at-edge@adobe.com`。

## 檢查LLM Optimizer中的路由狀態 {#verify-routing-status-in-ui}

您也可以在 LLM Optimizer 使用者介面中確認流量路由的狀態。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

![將最佳化部署到AI代理程式 — 已完成](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

## 返回總覽 {#return-to-overview}

若要進一步瞭解Edge最佳化，包括可用的機會、自動最佳化工作流程和常見問答，請返回[Edge最佳化概覽](/help/dashboards/optimize-at-edge/overview.md)。
