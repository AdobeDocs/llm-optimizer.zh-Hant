---
title: 存取控制
description: 了解在 Adobe LLM Optimizer 中產品指派和組織使用者的差異、唯讀使用者在 UI 中看到的內容，以及管理員如何在 Adobe Admin Console 中指派存取權。
feature: Customer Configuration
source-git-commit: 3b792a8ca7efd4b6d6764d2e83f9b0c103a56558
workflow-type: ht
source-wordcount: '618'
ht-degree: 100%

---


# 存取控制

Adobe LLM Optimizer 根據使用者人物誌支援基本存取控制。此功能僅供&#x200B;**付費客戶**&#x200B;使用，並按要求啟用。試用版客戶無法使用此功能。

>[!IMPORTANT]
>
>若要請求此功能的存取權，付費客戶應聯絡其 Adobe 帳戶經理。

## 產品指派的使用者 {#product-assigned-users}

如果您被指派至產品，將擁有與標準組織使用者相同的功能，而且具備以下權限：

* 對於[客戶設定](/help/dashboards/customer-configuration.md)中的提示、類別、主題和相關設定，擁有寫入權限。
* 部署[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)及管理建議。
* 管理 Google Search Console 設定。
* 管理邊緣最佳化和內容傳遞網路設定。
* 將新網站上線。

## 組織使用者 {#organizational-users}

組織使用者是&#x200B;**未**&#x200B;指派給產品的標準使用者。如果您是組織使用者，則您擁有 [LLM Optimizer 儀表板](/help/dashboards/dashboards-overview.md)和相關視圖的&#x200B;**唯讀**&#x200B;存取權。以下限制適用。

### 客戶設定 {#customer-configuration-restrictions}

* **上傳提示**&#x200B;已停用。
* 提示、類別、主題和區域的管理和編輯功能已停用。

  ![唯讀使用者的客戶設定限制](/help/dashboards/assets/access-control-customer-configuration.png)

### 內容傳遞網路設定 (客戶設定) {#cdn-configuration-restrictions}

* **內容傳遞網路上線**&#x200B;功能已停用 (唯讀使用者無法新增內容傳遞網路提供者)。
* **刪除內容傳遞網路**&#x200B;功能已停用 (唯讀使用者無法移除現有的內容傳遞網路設定)。
* 內容傳遞網路上線對話框中的&#x200B;**「提交」**&#x200B;按鈕已停用 (唯讀使用者無法完成內容傳遞網路設定)。

  ![唯讀使用者的內容傳遞網路設定限制](/help/dashboards/assets/access-control-cdn-configuration.png)

### 品牌存在感 — 資料洞察 {#brand-presence-restrictions}

* 主題旁邊的&#x200B;**「刪除」**&#x200B;按鈕已隱藏 (唯讀使用者無法將主題從追蹤清單中移除)。
* 提示旁邊的&#x200B;**「刪除」**&#x200B;按鈕已隱藏 (唯讀使用者無法將提示從追蹤清單中移除)。

  ![對唯讀使用者隱藏品牌存在感動作](/help/dashboards/assets/access-control-brand-presence.png)

### 代理式流量機會 (錯誤頁面機會) {#agentic-opportunities}

針對 404、403 和 503 錯誤頁面一類的機會：

* **部署最佳化**&#x200B;已隱藏。
* 資訊性警示說明需要部署存取權。

  ![在代理式流量機會上隱藏的部署最佳化](/help/dashboards/assets/access-control-agentic-deploy.png)

### 其他機會頁面 {#other-opportunities}

唯讀行為也適用於機會類型，例如：

* 目錄
* 摘要
* 可讀性
* 預轉譯
* 標題
* 常見問題
* 缺少結構化資料
* 一般修補機會

針對這些頁面：

* 當使用者沒有部署存取權時，**部署最佳化**&#x200B;會隱藏。
* 內嵌資訊性警示說明需要部署存取權。其訊息與以下內容類似：*需要部署存取權：您沒有部署最佳化或管理建議的權限。請聯絡您的管理員請求提供存取權。*
* 具有部署動作的固定式底部列已隱藏。

  ![需要部署存取權時顯示的內嵌警報](/help/dashboards/assets/access-control-deploy-alert.png)

  ![邊緣最佳化部署動作對唯讀使用者隱藏](/help/dashboards/assets/access-control-optimize-at-edge.png)

### Google Search Console 提示設定 {#gsc-restrictions}

* 管理和連線動作已停用或隱藏。
* 用於新增提示的動作欄已隱藏。

  ![Google Search Console 設定限制](/help/dashboards/assets/access-control-gsc.png)

### 將新網站上線 {#onboarding-restrictions}

* 對於沒有存取控制的使用者，停用將新網站上線的功能。

  ![已停用將新網站上線的功能](/help/dashboards/assets/access-control-onboarding.png)

## 將產品指派給使用者或群組 {#assign-product}

您組織的&#x200B;**系統管理員**&#x200B;可以使用 [Adobe Admin Console](https://adminconsole.adobe.com/) 將 Adobe LLM Optimizer 指派給使用者或群組。

1. 使用具有組織管理權限的帳戶登入 [Adobe Admin Console](https://adminconsole.adobe.com/)。
1. 將 Adobe LLM Optimizer 產品輪廓 (或是您的組織對應的產品權益) 指派給應獲得產品指派功能的使用者或群組。

如需詳細步驟，請參閱[在 Admin Console 中管理產品](https://helpx.adobe.com/tw/enterprise/using/manage-products.html)和[管理使用者群組](https://helpx.adobe.com/tw/enterprise/using/user-groups.html)。

>[!NOTE]
>
>Adobe Admin Console 不同發行版本之間的畫面流程可能會變更。如果上述選項與您的主控台不符，請使用 Adobe Admin Console 中的產品內說明連結，或聯絡您的 Adobe 帳戶團隊。
