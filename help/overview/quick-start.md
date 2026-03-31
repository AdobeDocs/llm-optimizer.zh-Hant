---
title: 快速入門
description: 瞭解如何入門品牌名稱和網域、從Experience Hub或Experience Cloud啟用試用版，以及完成Adobe LLM Optimizer設定。
feature: Quickstart, Onboarding
source-git-commit: dcbeb1c61dd9dcefd83908f65f8303d36c0fb78e
workflow-type: tm+mt
source-wordcount: '1208'
ht-degree: 50%

---


# 快速入門

若要開始使用LLM Optimizer，您必須完成入門流程。 上線後，您將能夠自訂類別、主題、提示和設定記錄轉送，以獲得更準確的見解，並完整存取[LLM Optimizer儀表板](/help/dashboards/dashboards-overview.md)和其他功能。

## 上線概觀

入門流程從入門您的網域和品牌名稱開始。 以下詳細說明了入門歷程的每個部分，以及有關如何儘快開始使用LLM Optimizer的實用提示。

### 允許 Adobe LLM Optimizer 存取公開頁面

為了提供精確的內容和技術建議，Adobe LLM Optimizer 需要存取您的公開頁面。 而這項操作是透過安全內部爬蟲 (Spacecat/1.0 使用者代理) 所完成。

設定要求：

* 將Spacecat/1.0使用者代理程式新增至您網站的robots.txt檔案或機器人流量管理規則中的允許清單。
* 請確定在網域或CDN層級未封鎖頁面。 被封鎖的頁面無法編入索引，意味著無法為這些頁面生成最佳化任務和洞察。

如果儀表板中顯示的內容能見度很低，請確認爬蟲可以存取您的網域。 存取權受限制是索引不完整的常見原因。

## 步驟1：加入您的品牌名稱和網域 {#step-1-onboard-your-domain}

若要開始使用LLM Optimizer，請先啟用您的試用版（如果適用），並加入您的品牌名稱和網域。

### 啟用您的試用版

啟用流程會依您的Adobe產品而有所不同。

#### AEM Cloud客戶

若要啟用試用版，身為AEM Cloud客戶，您可以：

* 導覽至[Experience Hub](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)，並使用產品公告卡啟用LLM Optimizer。 選取&#x200B;**試用LLM Optimizer**&#x200B;後，您會重新導向至[https://llmo.now](https://llmo.now)。 透過IMS登入，然後輸入網域和品牌名稱以開始入門流程。
* 或直接移至[https://llmo.now](https://llmo.now)並登入。

![LLM Optimizer 試用版](/help/overview/assets/llm-trial.png)

#### Adobe Analytics客戶

如果您是Adobe Analytics客戶，將會在Experience Cloud首頁上看到橫幅。

![Experience Cloud首頁包含開始您的Adobe LLM Optimizer試用橫幅](/help/overview/assets/experience-cloud-llmo-trial-banner.png)

您可以透過下列其中一種方式來啟用試用版：

* 在橫幅中選取&#x200B;**開始您的Adobe LLM Optimizer試用版**。
* 直接移至[https://llmo.now](https://llmo.now)並登入。

試用版生效後，請繼續上線您的品牌名稱和網域。

>[!NOTE]
>
> * **免費試用版：** AEM Cloud和Adobe Analytics客戶可以使用LLM Optimizer的免費試用版。
> * **在2026年4月1日或之後啟用試用版的客戶**&#x200B;最多可使用100個提示、一個網域，以及針對單一機會型別最多在10個URL中部署最佳化。
> * **在2026年4月1日之前啟用試用版的客戶**&#x200B;可繼續根據其現有條款存取最多200個提示。
>
>使用超出包含限制需要單獨的授權協定。 存取權係依「現況」及「可用狀態」提供，並可隨時修改、限制或移除。 如需詳細資訊，請聯絡您的客戶代表。

#### 加入您的品牌名稱和網域

開始使用LLM Optimizer的品牌名稱和網域。

1. 輸入您的品牌名稱和相關聯的網域。

   * 這應該是您要分析和最佳化內容的主要網域。

1. 完成入門。

   * 提交後，LLM Optimizer就會開始分析您的網域並產生深入分析。

![LLM Optimizer 網域](/help/overview/assets/domain.png)

>[!NOTE]
>新增加的提示在處理完成前，不會顯示在[品牌存在感儀表板](/help/dashboards/brand-presence.md)中。

>[!NOTE]
>您提供的網域將供組織中所有人使用，且無法變更。

在上線階段將生成一組少量的類別、主題和提示。 您的網站上線後不久，即可獲取針對這些提示所做的品牌存在感分析。

您也可以在Edge部署最佳化。 深入瞭解[在Edge最佳化 — 常見問題](https://experienceleague.adobe.com/zh-hant/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions)。

此外，設定[CDN記錄轉送](#step-4)以進行流量分析。 LLM Optimizer需要來自代理和引薦流量的品牌存在感資料和深入分析，以識別機會並提供可提升AI可見度的規範性建議。

### 非AEM Cloud客戶

貴組織完成商業合約後，您會使用您組織選取的網域加入LLM Optimizer。 上線完成時，請登入[https://llmo.now](https://llmo.now)。

## 第 2 步：自訂類別、主題和提示

您的網站上線後，您便可以檢視根據上線階段自動生成的一小組提示進行的品牌存在感分析。 往後，您可以自訂品牌的類別、主題和提示。 此設定是在[客戶設定儀表板](/help/dashboards/customer-configuration.md)上建立的。

![客戶設定儀表板](/help/overview/assets/prompt-creation.png)

您可以透過此儀表板：

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

## 第 3 步：品牌存在感洞察

您的網域上線後，您將會在品牌存在感視圖中看到根據上線期間自動生成的提示所獲取的初步洞察。 在您自訂自己的類別、主題和提示後，LLM Optimizer 將根據您提供的提示自動觸發品牌存在感分析，並在 24 小時內提供結果。

## 第 4 步：提供內容傳遞網路記錄轉送的相關資訊 {#step-4}

若要解除鎖定代理流量和引薦流量深入分析，請從[客戶設定儀表板](/help/dashboards/customer-configuration.md#cdn-configuration)新增CDN記錄轉送資訊。 開啟&#x200B;**CDN設定**&#x200B;標籤，並選取&#x200B;**內建CDN**。

![客戶設定內容傳遞網路](/help/overview/assets/cc-cdn.png)

或者，如果並未事先新增任何內容傳遞網路提供者 (如上所述)，則在您首次存取代理式和轉介流量儀表板時，系統會提示您新增內容傳遞網路記錄轉送。 如需更多詳細資料，請參閱：

* [代理式流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [轉介流量](/help/dashboards/referral-traffic.md#setup)

>[!NOTE]
>關於使用客戶管理的內容傳遞網路 (BYOCDN) 時進行記錄轉送的詳細資訊，請參閱 [BYOCDN 記錄轉送概觀](/help/overview/log-forwarding/log-forwarding-overview.md)

## 第 5 步：探索儀表板並採取行動

在提供內容傳遞網路記錄轉送的相關資訊後，您可以：

* 檢視[品牌存在感](/help/dashboards/brand-presence.md)儀表板，並查看您的能見度分數及追蹤您與其他品牌相比的效能。
* 如果已經設定CDN記錄轉送，請探索[代理](/help/dashboards/agentic-traffic.md)和[引薦流量](/help/dashboards/referral-traffic.md)儀表板。
* 使用[機會](/help/dashboards/opportunities.md)來找出需要改善的內容與技術項目。
* 匯出資料並與您的團隊協作，或邀請您的同事使用產品。

最後，若要完全了解 LLM Optimizer 的功能，您應該探索所有可用的[儀表板](/help/dashboards/dashboards-overview.md)。
