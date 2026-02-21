---
title: 快速入門
description: 開始使用 Adobe LLM Optimizer，讓您的品牌上線、獲取 AI 能見度洞察，並探索儀表板以提高搜尋效能。
feature: Quickstart, Onboarding
source-git-commit: 82830e66d43ddd9741617cdf6daab63cd259554b
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 93%

---


# 快速入門

若要開始使用LLM Optimizer，您必須完成入門流程，如下列步驟所述。 完成上線流程後，您將擁有 [LLM Optimizer 儀表板](/help/dashboards/dashboards-overview.md)和其他功能的完整存取權。

## 上線概觀

上線流程從您將網域上線開始。 依據您是不是 AEM Cloud 的客戶而定，將會執行不同的流程。 完成流程後，您必須提供內容傳遞網路記錄轉送的相關資訊，並於最後自訂類別、主題和提示。 流程的各個部分於下文詳細說明，並提供有關如何儘快開始使用 LLM Optimizer 的實用提示。

### 允許 Adobe LLM Optimizer 存取公開頁面

為了提供精確的內容和技術建議，Adobe LLM Optimizer 需要存取您的公開頁面。 而這項操作是透過安全內部爬蟲 (Spacecat/1.0 使用者代理) 所完成。

設定要求：

* 在您的網站 robots.txt 檔案或機器人流量管理規則的允許清單中，加入 Spacecat/1.0 使用者代理
* 請確認在網域或內容傳遞網路層級並未封鎖頁面。 被封鎖的頁面無法編入索引，意味著無法為這些頁面生成最佳化任務和洞察。

如果儀表板中顯示的內容能見度很低，請確認爬蟲可以存取您的網域。 存取權受限制是索引不完整的常見原因。

## 第 1 步：將您的網域上線

### 先試用再購買

AEM Cloud (Cloud Service、Managed Services、Edge Delivery Service)客戶可選擇使用&#x200B;**購買前先試用**&#x200B;優惠。 這是 LLM Optimizer 免費試用版，提供最多 200 個免費提示。 使用 200 個以上的提示需有單獨的授權合約。 依「現狀」及「現可提供」之基礎提供存取權，並可由 Adobe 隨時修改、限制或移除。

免費版本並未提供部分產品功能：

* 試用版僅限一個網域使用。 完成設定後，您就無法變更所提供的網域。
* 部署最佳化的功能可在搶先使用中取得。 如需詳細資訊，請參閱[在Edge中最佳化常見問題](https://experienceleague.adobe.com/zh-hant/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions)。

關於如何啟用免費試用版並將您的網域上線的詳細資訊，請參閱以下區段。

### AEM Cloud 客戶

如果您是 AEM Cloud 客戶，您可以選擇在 [Experience Hub](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/experience-hub/experience-hub) 中使用產品公告卡來試用 LLM Optimizer。

>[!NOTE]
>新增加的提示在處理完成前，不會顯示在[品牌存在感儀表板](/help/dashboards/brand-presence.md)中。 AEM Cloud 客戶可使用 LLM Optimizer 免費試用版。 使用 200 個以上的提示需有單獨的授權合約。 依「現狀」及「現可提供」之基礎提供存取權，並可由 Adobe 隨時修改、限制或移除。 如需詳細資訊，請聯絡您的帳戶代表。

![LLM Optimizer 試用版](/help/overview/assets/llm-trial.png)

按一下「**試用 LLM Optimizer**」按鈕後，便會將您重新導向至 [https://llmo.now](https://llmo.now)。 接著您必須透過 IMS 登入。 登入後，提供一個網域和品牌名稱，便可以啟動上線流程。

![LLM Optimizer 網域](/help/overview/assets/domain.png)

>[!NOTE]
>您提供的網域將供組織中所有人使用，且無法變更。

在上線階段將生成一組少量的類別、主題和提示。 您的網站上線後不久，即可獲取針對這些提示所做的品牌存在感分析。

<!--![Brand Presence Analysis](/help/overview/assets/bp-analysis.png)-->

此外，您也需要設定[內容傳遞網路記錄轉送](#step-4)以利進行流量分析。 LLM Optimizer 需要利用代理式和轉介流量的品牌存在感資料和洞察來確認機會及提供指示性建議，以利提高 AI 能見度。

### 非 AEM Cloud 客戶

業務合約定案後，您將透過要在 LLM Optimizer 上線的網域完成上線流程。 完成此上線流程後，您將能夠透過 [https://llmo.now](https://llmo.now) 登入 LLM Optimizer。

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

若要獲取代理式流量和轉介流量的洞察，您必須提供內容傳遞網路記錄轉送的相關資訊。 導覽至「**內容傳遞網路設定**」分頁標籤，然後按一下「**內建內容傳遞網路**」，即可從[客戶設定儀表板](/help/dashboards/customer-configuration.md#cdn-configuration)新增這些資訊。

![客戶設定內容傳遞網路](/help/overview/assets/cc-cdn.png)

或者，如果並未事先新增任何內容傳遞網路提供者 (如上所述)，則在您首次存取代理式和轉介流量儀表板時，系統會提示您新增內容傳遞網路記錄轉送。 如需更多詳細資料，請參閱：

* [代理式流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [轉介流量](/help/dashboards/referral-traffic.md#setup#setup)

## 第 5 步：探索儀表板並採取行動

在提供內容傳遞網路記錄轉送的相關資訊後，您可以：

* 檢視[品牌存在感](/help/dashboards/brand-presence.md)儀表板，並查看您的能見度分數及追蹤您與其他品牌相比的效能。
* 如果已經設定內容傳遞網路記錄轉送，請探索[代理式](/help/dashboards/agentic-traffic.md)和[轉介流量](/help/dashboards/referral-traffic.md)儀表板。
* 使用[機會](/help/dashboards/opportunities.md)來找出需要改善的內容與技術項目。
* 匯出資料並與您的團隊協作，或邀請您的同事使用產品。

最後，若要完全了解 LLM Optimizer 的功能，您應該探索所有可用的[儀表板](/help/dashboards/dashboards-overview.md)。
