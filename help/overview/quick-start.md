---
title: 快速啟動
description: 開始使用Adobe LLM Optimizer — 加入您的品牌、解鎖AI可見度深入分析，並探索儀表板以提高搜尋效能。
feature: Quickstart, Onboarding
source-git-commit: 24183fbe2577bb9402f8b6aaaf1e46c75403383d
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 0%

---


# 快速啟動

若要開始使用LLM Optimizer，您必須完成上線流程，如下列步驟所詳述。 完成程式後，您將擁有[LLM Optimizer儀表板](/help/dashboards/dashboards-overview.md)和其他功能的完整存取權。

## 入門概觀

上線流程從上線您的網域開始。 此程式不盡相同，端視您是否為AEM Cloud客戶而定。 完成程式後，您需要提供CDN記錄轉送的資訊，以及最後自訂類別、主題和提示。 此程式的各個部分於下文詳細說明，並提供有關如何儘快開始使用LLM Optimizer的實用提示。

### 允許Adobe LLM Optimizer存取公開頁面

為了提供精確的內容和技術建議，Adobe LLM Optimizer需要存取您的公開頁面。 這是透過安全內部爬蟲（Spacecat/1.0使用者代理）完成的。

設定需求：

* 將Spacecat/1.0使用者代理程式新增至您網站robots.txt檔案或機器人流量管理規則中的允許清單
* 請確定在網域或CDN層級未封鎖頁面。 封鎖的頁面無法編制索引，這表示無法為其產生最佳化任務和深入分析。

如果內容可見度在控制面板中看起來很低，請確認爬蟲可以存取您的網域。 限制存取是不完整索引的常見原因。

## 步驟1：將您的網域上線

### 購買前先試一次

AEM Cloud (Cloud Service、Managed Services、Edge Delivery Service)客戶可選擇使用&#x200B;**購買前先試用**&#x200B;優惠。 這是免費試用版的LLM Optimizer，最多可提供200個免費提示。 使用200個以上的提示需要單獨的授權合約。 存取權係依「現況」及「可用性」提供，並可由Adobe隨時修改、限制或移除。

有些產品功能並非免費版本提供：

* 試用版僅限於一個網域。 完成設定後，您將無法變更您提供的網域。
* 將不支援部署最佳化。

請參閱以下章節，以取得如何啟用免費試用版並將您的網域上線的詳細資訊。

### AEM Cloud客戶

如果您是AEM Cloud客戶，您可以選擇在[Experience Hub](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)中使用產品公告卡來試用LLM Optimizer。

>[!NOTE]
>處理完成前，新新增的提示不會出現在[品牌存在感儀表板](/help/dashboards/brand-presence.md)中。 AEM Cloud客戶可使用免費試用版的LLM Optimizer。 使用200個以上的提示需要單獨的授權合約。 存取權係依「現況」及「可用性」提供，並可由Adobe隨時修改、限制或移除。 如需詳細資訊，請洽詢您的客戶代表。

![LLM Optimizer試用版](/help/overview/assets/llm-trial.png)

按一下&#x200B;**試用LLM Optimizer**&#x200B;按鈕後，您將被重新導向至[https://llmo.now](https://llmo.now)。 接著，您需要透過IMS登入。 登入後，您將透過提供網域和品牌名稱來開始入門流程。

![LLM Optimizer網域](/help/overview/assets/domain.png)

>[!NOTE]
>您提供的網域將由您組織的每個人使用，且無法變更。

在上線階段將生成一小部分類別、主題和提示。 您的網站上線後不久，即可對這些提示進行品牌存在感分析。

<!--![Brand Presence Analysis](/help/overview/assets/bp-analysis.png)-->

此外，您還需要設定[CDN記錄轉送](#step-4)以進行流量分析。 LLM Optimizer需要來自代理和引薦流量的品牌存在感資料和深入分析，以識別機會並提供規範性建議，從而提高AI可見度。

### 非AEM雲端客戶

業務合約完成後，您將加入LLM Optimizer上要加入的網域。 此上線完成後，您將能夠透過[https://llmo.now](https://llmo.now)登入LLM Optimizer。

### 步驟2：自訂類別、主題和提示

您的網站上線後，您可以根據上線階段自動產生的一小組提示來檢視品牌存在感分析。 往後，您可以自訂品牌的類別、主題和提示。 此設定是在[客戶設定儀表板](/help/dashboards/customer-configuration.md)上建立。

![客戶設定儀表板](/help/overview/assets/prompt-creation.png)

從此控制面板，您可以：

* 新增符合您業務優先順序的&#x200B;**新類別**。 類別可以是與您的網域相關的廣泛內容區域。
* 輸入您想要追蹤的&#x200B;**自訂主題**&#x200B;或子主題。 主題可以是與您的領域關聯的大量非品牌關鍵字相連的特定主題。
* 建立&#x200B;**您的提示**&#x200B;以監視特定查詢中的可見性。 提示是提供基線可見性的查詢（品牌和非品牌）。 系統只會根據您提供的類別和主題自動產生有限數量的提示。
* 定義提及&#x200B;**別名**，以確保擷取並計入品牌的所有提及。
* 定義&#x200B;**其他別名**&#x200B;以精確追蹤其他品牌。

>[!NOTE]
>您詢問LLM的確切提示不會公開提供，因為LLM不會公開這些提示。

>[!NOTE]
>
> 如需有關如何設定類別、主題、提示的詳細資訊，請參閱[設定類別、主題、提示的最佳實務](/help/overview/best-practices-topics-prompts.md)頁面。

### 步驟3：品牌存在感深入分析

您的網域上線後，您將根據上線期間自動產生的提示，在品牌存在感檢視中看到初始見解。 一旦您自訂自己的類別、主題和提示，LLM Optimizer就會根據您提供的提示自動觸發品牌存在感分析，結果將在24小時內可用。

### 步驟4：提供CDN記錄轉送的資訊 {#step-4}

若要解除鎖定代理流量和引薦流量深入分析，您需要為CDN記錄轉送提供資訊。 您可以導覽至「[CDN設定](/help/dashboards/customer-configuration.md#cdn-configuration)」標籤，然後按一下「**內建CDN**」，從「**客戶設定」儀表板**&#x200B;新增此專案。

![客戶組態CDN](/help/overview/assets/cc-cdn.png)

或者，如果事先沒有新增CDN提供者（如上所述），則在第一次存取代理和引薦流量儀表板時，系統會提示您新增CDN記錄轉送。 如需詳細資訊，請參閱：

* [代理式流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [轉介流量](/help/dashboards/referral-traffic.md#setup#setup)

### 步驟5：探索儀表板並採取行動

提供CDN記錄轉送的資訊後，您可以：

* 檢視[品牌存在感](/help/dashboards/brand-presence.md)儀表板，並檢視您的可見度分數和追蹤您相對於其他品牌的績效。
* 如果已經設定CDN記錄轉送，請探索[代理程式](/help/dashboards/agentic-traffic.md)和[引薦流量](/help/dashboards/referral-traffic.md)儀表板。
* 使用[機會](/help/dashboards/opportunities.md)來識別內容與技術改進。
* 匯出資料並與您的團隊共同作業，或邀請您的同事使用產品。

最後，若要完全瞭解LLM Optimizer的功能，您應該探索所有可用的[控制面板](/help/dashboards/dashboards-overview.md)。
