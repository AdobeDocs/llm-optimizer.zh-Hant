---
title: 存取控制
description: 瞭解產品指派的使用者和組織使用者在Adobe LLM Optimizer中的差異、唯讀使用者在UI中看到的內容，以及管理員如何在Adobe Admin Console中指派存取權。
feature: Customer Configuration
source-git-commit: 3b792a8ca7efd4b6d6764d2e83f9b0c103a56558
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 4%

---


# 存取控制

Adobe LLM Optimizer根據使用者角色支援基本存取控制。 此功能僅供&#x200B;**付費客戶**&#x200B;使用，並已依要求啟用。 試用版客戶無法使用此功能。

>[!IMPORTANT]
>
>若要要求存取此功能，付費客戶應連絡其Adobe客戶經理。

## 產品指派的使用者 {#product-assigned-users}

如果您被指派至產品，則除了以下許可權外，您還可擁有與標準組織使用者相同的權能：

* 在[客戶組態](/help/dashboards/customer-configuration.md)中寫入提示、類別、主題和相關設定的存取權。
* 部署[在Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)並管理建議。
* 管理Google Search Console設定。
* 在Edge和CDN設定中管理最佳化。
* 加入新網站。

## 組織使用者 {#organizational-users}

組織使用者是&#x200B;**未**&#x200B;指派給產品的標準使用者。 如果您是組織使用者，則您擁有[LLM Optimizer儀表板](/help/dashboards/dashboards-overview.md)和相關檢視的&#x200B;**唯讀**&#x200B;存取權。 以下限制適用。

### 客戶設定 {#customer-configuration-restrictions}

* **上載提示**&#x200B;已停用。
* 已停用管理和編輯提示、類別、主題和區域。

  ![唯讀使用者的客戶組態限制](/help/dashboards/assets/access-control-customer-configuration.png)

### CDN設定（客戶設定） {#cdn-configuration-restrictions}

* **內建CDN**&#x200B;已停用（唯讀使用者無法新增CDN提供者）。
* **刪除CDN**&#x200B;已停用（唯讀使用者無法移除現有的CDN設定）。
* CDN內建對話方塊中的&#x200B;**提交**&#x200B;按鈕已停用（唯讀使用者無法完成CDN設定）。

  唯讀使用者的![CDN組態限制](/help/dashboards/assets/access-control-cdn-configuration.png)

### 品牌存在感 — 資料深入分析 {#brand-presence-restrictions}

* 主題旁的&#x200B;**刪除**&#x200B;按鈕已隱藏（唯讀使用者無法從追蹤中移除主題）。
* 提示旁邊的&#x200B;**刪除**&#x200B;按鈕已隱藏（唯讀使用者無法從追蹤移除提示）。

  針對唯讀使用者隱藏的![品牌存在感動作](/help/dashboards/assets/access-control-brand-presence.png)

### 代理流量機會（錯誤頁面機會） {#agentic-opportunities}

若為機會，例如404、403和503錯誤頁面：

* **部署最佳化**&#x200B;已隱藏。
* 資訊性警報說明需要部署存取權。

  ![隱藏在代理流量機會上的部署最佳化](/help/dashboards/assets/access-control-agentic-deploy.png)

### 其他機會頁面 {#other-opportunities}

唯讀行為也適用於機會型別，例如：

* 目錄
* 摘要
* 可讀性
* 預轉譯
* 標題
* 常見問題
* 遺失結構化資料
* 一般修補機會

針對這些頁面：

* 當使用者沒有部署存取權時，**部署最佳化**&#x200B;會隱藏。
* 內嵌警示說明需要部署存取權。 此訊息類似： *需要部署存取權 — 您沒有部署最佳化或管理建議的許可權。 請聯絡您的管理員以要求存取權。*
* 具有部署動作的粘性底部列已隱藏。

  ![需要部署存取權時內嵌警報](/help/dashboards/assets/access-control-deploy-alert.png)

  ![在唯讀使用者隱藏的Edge部署動作最佳化](/help/dashboards/assets/access-control-optimize-at-edge.png)

### Google搜尋控制檯提示設定 {#gsc-restrictions}

* 管理和連線動作已停用或隱藏。
* 用於新增提示的動作欄已隱藏。

  ![Google Search Console設定限制](/help/dashboards/assets/access-control-gsc.png)

### 載入新網站 {#onboarding-restrictions}

* 沒有存取控制的使用者無法上線新網站。

  ![已停用新站台](/help/dashboards/assets/access-control-onboarding.png)

## 將產品指派給使用者或群組 {#assign-product}

您組織的&#x200B;**系統管理員**&#x200B;可以使用[Adobe Admin Console](https://adminconsole.adobe.com/)將Adobe LLM Optimizer指派給使用者或群組。

1. 使用為您的組織具有管理許可權的帳戶登入[Adobe Admin Console](https://adminconsole.adobe.com/)。
1. 將Adobe LLM Optimizer產品設定檔（或您組織的對等產品權利）指派給應接收產品指派權能的使用者或群組。

如需詳細步驟，請參閱[在Admin Console中管理產品](https://helpx.adobe.com/tw/enterprise/using/manage-products.html)和[管理使用者群組](https://helpx.adobe.com/tw/enterprise/using/user-groups.html)。

>[!NOTE]
>
>Adobe Admin Console中的熒幕流程在發行版本之間可能會變更。 如果上述選項與您的主控台不符，請使用Adobe Admin Console中的產品內說明連結，或聯絡您的Adobe客戶團隊。
