---
title: 快速入門
description: 了解如何將您的品牌名稱和網域上線、從 Experience Hub 或 Adobe Experience Cloud 啟用試用版，以及完成 Adobe LLM Optimizer 設定。
feature: Quickstart, Onboarding
source-git-commit: 82bfbea9375fe7f3cd7f27dfdf0adcab6489731d
workflow-type: tm+mt
source-wordcount: '1463'
ht-degree: 76%

---


# 快速入門

若要開始使用LLM Optimizer，請完成入門流程。 然後，自訂類別、主題和提示，設定CDN記錄轉送並開啟[儀表板](/help/dashboards/dashboards-overview.md)以獲得更完整的深入分析。

<!--Where steps differ by layout, use **Customer Configuration (classic experience)** or **Brands Management** / **Prompts Management**, whichever matches your current interface.-->

## 以品牌為中心的體驗 {#brand-centric-experience}

依預設，新客戶一開始會使用具有入門導向設定且重點突出、品牌優先的介面。 在這個新介面中，每個組織都從可用的品牌和其他建議品牌開始，以供選擇。 LLM Optimizer現有客戶將逐漸改用這種以品牌為中心的體驗。

## 上線概觀

上線程序從您將網域和品牌名稱上線開始。 上線歷程的各部分將於下文詳細說明，並提供實用提示，協助您儘快開始使用 LLM Optimizer。

### 允許 Adobe LLM Optimizer 存取公開頁面

為了提供精確的內容和技術建議，Adobe LLM Optimizer 需要存取您的公開頁面。 而這項操作是透過安全內部爬蟲 (Spacecat/1.0 使用者代理) 所完成。

設定要求：

* 在網站的 robots.txt 檔案或機器人流量管理規則的允許清單中，加入 Spacecat/1.0 使用者 AI 代理。
* 請確認在網域或內容傳遞網路層級皆未封鎖相關頁面。 被封鎖的頁面無法編入索引，意味著無法為這些頁面生成最佳化任務和洞察。

如果儀表板中顯示的內容能見度很低，請確認爬蟲可以存取您的網域。 存取權受限制是索引不完整的常見原因。

## 步驟 1：將您的品牌名稱和網域上線 {#step-1-onboard-your-domain}

若要開始使用 LLM Optimizer，請先啟用您的試用版 (如適用)，並將您的品牌名稱和網域上線。

### 啟用試用版

依您的 Adobe 產品而定，啟用流程會有所不同。

#### AEM Cloud 客戶

若要啟用試用版，您身為 AEM Cloud 客戶，可以執行下列動作：

* 導覽至 [Experience Hub](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)，並使用產品公告卡片啟用 LLM Optimizer。 選取「**試用 LLM Optimizer**」後，會重新導向至 [https://llmo.now](https://llmo.now)。 透過 IMS 登入，然後輸入網域和品牌名稱，開始上線程序。
* 或者直接前往 [https://llmo.now](https://llmo.now) 並登入。

![LLM Optimizer 試用版](/help/overview/assets/llm-trial.png)

#### Adobe Analytics 客戶

如果您是 Adobe Analytics 客戶，會在 Adobe Experience Cloud 首頁上看到橫幅。

![Adobe Experience Cloud 首頁上有「開始使用您的 Adobe LLM Optimizer 試用版」橫幅](/help/overview/assets/experience-cloud-llmo-trial-banner.png)

您可以透過下列其中一種方式啟用試用版：

* 在橫幅中選取「**開始使用您的 Adobe LLM Optimizer 試用版**」。
* 直接前往 [https://llmo.now](https://llmo.now) 並登入。

試用版啟用後，開始將您的品牌名稱和網域上線。

>[!NOTE]
>
> * **免費試用版：** AEM Cloud 和 Adobe Analytics 客戶可以使用 LLM Optimizer 的免費試用版。
> * **在 2026 年 4 月 1 日當天或之後啟用試用版的客戶，**&#x200B;可使用最多 100 個提示、一個網域，以及針對單一機會類型在最多 10 個 URL 中部署最佳化。
> * **在 2026 年 4 月 1 日之前啟用試用版的客戶，**&#x200B;根據其現有條款，可以繼續存取最多 200 個提示。
>
>超出上限的使用量需有另外的授權合約。 依「現狀」及「現可提供」之基礎提供存取權，並可隨時修改、限制或移除。 如需詳細資訊，請聯絡您的帳戶代表。

#### 將您的品牌名稱和網域上線

將您的品牌名稱和網域上線，以便開始使用 LLM Optimizer。

1. 輸入您的品牌名稱和關聯的網域。

   * 這個網域應是您想要分析和最佳化內容的主要網域。

1. 完成上線。

   * 提交後，LLM Optimizer 便會開始分析您的網域並產生洞察。

![LLM Optimizer 網域](/help/overview/assets/domain.png)

>[!NOTE]
>新增加的提示在處理完成前，不會顯示在[品牌存在感儀表板](/help/dashboards/brand-presence.md)中。

>[!NOTE]
>您提供的網域將供組織中所有人使用，且無法變更。

在上線階段將生成一組少量的類別、主題和提示。 您的網站上線後不久，即可獲取針對這些提示所做的品牌存在感分析。

也可以在邊緣架構部署最佳化。 若要了解更多相關資訊，請參閱[邊緣架構最佳化 — 常見問題](https://experienceleague.adobe.com/zh-hant/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions)。

此外，若要進行流量分析，請設定[內容傳遞網路記錄轉寄](#step-4)。 LLM Optimizer 需要利用代理式和轉介流量的品牌存在感資料與洞察，才能確認機會並提供能提高 AI 能見度的規範性建議。

### 非 AEM Cloud 客戶

您組織的業務合約定案後，您便會以組織選取的網域上線至 LLM Optimizer。 上線完成後，請登入 [https://llmo.now](https://llmo.now)。

## 第 2 步：自訂類別、主題和提示 {#step-2-customize-categories-topics-and-prompts}

您的網站上線後，您便可以檢視根據上線階段自動生成的一小組提示進行的品牌存在感分析。 往後，您可以自訂品牌的類別、主題和提示。

### 客戶設定（傳統導覽）

如果您使用傳統導覽（不是以品牌為中心的體驗），您可以從[客戶設定儀表板](/help/dashboards/customer-configuration.md)自訂品牌的類別、主題和提示。

![客戶設定儀表板](/help/overview/assets/prompt-creation.png)

從「客戶組態」控制面板，您可以：

* 新增符合您的業務優先順序的&#x200B;**新類別**。 類別可以是與您的網域相關的廣泛的內容領域。
* 輸入您要追蹤的&#x200B;**自訂主題**&#x200B;或子主題。 主題可以是與您的網域相關聯的高搜尋量非品牌關鍵字緊密結合的特定主題。
* 建立&#x200B;**您的提示**&#x200B;以監視特定查詢中的能見度。 提示是提供基準能見度的查詢 (品牌和非品牌)。 系統只會根據您提供的類別和主題自動生成有限數量的提示。
* 定義提及&#x200B;**別名**，以確保擷取並計入提及品牌的所有情形。
* 定義&#x200B;**其他別名**&#x200B;以便精準追蹤其他品牌。

>[!NOTE]
>您詢問 LLM 的確切提示並不會對外公開，因為 LLM 並未揭露這些提示。

>[!NOTE]
>
> 如需如何設定類別、主題、提示的詳細資訊，請參閱[設定類別、主題、提示的最佳做法](/help/overview/best-practices-topics-prompts.md)頁面。

### 以品牌為中心的體驗類別、主題和提示

對於使用以品牌為中心的體驗的客戶，您可以新增類別、主題和提示，如下所示：

* **類別** — 瀏覽至&#x200B;**品牌管理**&#x200B;並按一下&#x200B;**類別**。 類別是在全球層級定義，並套用至「品牌管理」下的所有品牌。

  在導覽中使用類別的![品牌管理](/help/assets/brand-centric-experience/llmo-app-shell.png)

* **主題和提示** — 瀏覽至&#x200B;**提示管理**&#x200B;以建立主題和提示，包括特定品牌的提示。

  ![提示管理](/help/assets/brand-centric-experience/prompts-management.png)

## 第 3 步：品牌存在感洞察

您的網域上線後，您將會在品牌存在感視圖中看到根據上線期間自動生成的提示所獲取的初步洞察。 在您自訂自己的類別、主題和提示後，LLM Optimizer 將根據您提供的提示自動觸發品牌存在感分析，並在 24 小時內提供結果。

>[!NOTE]
>
> 對於使用品牌導向體驗的客戶，請導覽至&#x200B;**品牌存在感**，然後選取您要檢視使用品牌下拉式清單品牌存在感的品牌。 您也可以使用此體驗在&#x200B;**所有品牌**&#x200B;層級檢視品牌可見度。

## 第 4 步：提供內容傳遞網路記錄轉送的相關資訊 {#step-4}

若要解除鎖定代理流量和引薦流量深入分析，請註冊CDN記錄轉送，讓LLM Optimizer可讀取您的存取記錄。

### 客戶設定（傳統導覽）

如果您使用傳統導覽，可以從[客戶設定儀表板](/help/dashboards/customer-configuration.md#cdn-configuration)新增CDN記錄轉送資訊。 開啟「**內容傳遞網路設定**」標籤，並選取「**上線內容傳遞網路**」。

![客戶設定內容傳遞網路](/help/overview/assets/cc-cdn.png)

或者，如果並未事先新增任何內容傳遞網路提供者 (如上所述)，則在您首次存取代理式和轉介流量儀表板時，系統會提示您新增內容傳遞網路記錄轉送。 如需更多詳細資料，請參閱：

* [代理式流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [轉介流量](/help/dashboards/referral-traffic.md#setup)

>[!NOTE]
>關於使用客戶管理的內容傳遞網路 (BYOCDN) 時進行記錄轉送的詳細資訊，請參閱 [BYOCDN 記錄轉送概觀](/help/overview/log-forwarding/log-forwarding-overview.md)

### 品牌導向體驗CDN記錄轉送

對於使用品牌導向體驗的客戶，您可以從&#x200B;**品牌管理**&#x200B;新增CDN記錄轉送資訊，如下所示：開啟&#x200B;**品牌管理**，然後按一下&#x200B;**CDN**&#x200B;標籤。

![品牌管理 — CDN記錄轉送](/help/assets/brand-centric-experience/brands-management-cdn.png)

## 第 5 步：探索儀表板並採取行動

在提供內容傳遞網路記錄轉送的相關資訊後，您可以：

* 檢視[品牌存在感](/help/dashboards/brand-presence.md)儀表板，並查看您的能見度分數及追蹤您與其他品牌相比的效能。
* 若已經設定內容傳遞網路記錄轉寄，請探索[代理式](/help/dashboards/agentic-traffic.md)和[轉介流量](/help/dashboards/referral-traffic.md)儀表板。
* 使用[機會](/help/dashboards/opportunities.md)來找出需要改善的內容與技術項目。
* 匯出資料並與您的團隊協作，或邀請您的同事使用產品。

>[!NOTE]
> 在品牌導向體驗中，從導覽區段到左側存取所需的檢視。

最後，若要完全了解 LLM Optimizer 的功能，您應該探索所有可用的[儀表板](/help/dashboards/dashboards-overview.md)。
