---
title: 快速開始
description: 這是快速入門文章。
source-git-commit: 5dbf794b87df92583daec83ab02063821ee7a412
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# 快速開始

若要開始使用LLM Optimizer，您必須完成上線流程。 之後，您將擁有LLM Optimizer儀表板的完整存取權和完整功能。

## 入門概觀

上線流程從上線您的網域開始。 此程式會因您是否為AEM Cloud客戶而有所不同。 完成程式後，您需要提供CDN記錄轉送的資訊，以及最後自訂類別、主題和提示。

### 步驟1：將您的網域上線

### AEM Cloud客戶

AEM Cloud客戶(Cloud Service/Managed Services/ Edge Delivery Service)將看到可透過[Experience Hub](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)中的產品公告卡試用LLM Optimizer的選項。

>[!NOTE]
>處理完成前，新新增的提示不會出現在品牌顯示中。 AEM Cloud客戶(Cloud Service、Managed Services/ Edge Delivery Service)可使用LLM Optimizer的免費試用版。 使用200個以上的提示需要單獨的授權合約。 存取權係依「現況」及「可用性」提供，並可由Adobe隨時修改、限制或移除。 如需詳細資訊，請洽詢您的[帳戶代表]。

![LLM Optimizer試用版](/help/overview/assets/llm-trial.png)

按一下&#x200B;**試用LLM Optimizer**&#x200B;按鈕後，系統會將您重新導向至[https://llmo.now](https://llmo.now) 。 接著，您需要透過IMS登入。 登入後，您將透過提供網域和品牌名稱來開始入門流程。

![LLM Optimizer網域](/help/overview/assets/domain.png)

>[!NOTE]
>您提供的網域將由您組織的每個使用者使用，且無法變更。

若要觸發品牌顯示狀態分析，您必須提供類別、主題和提示。

![品牌狀態分析](/help/overview/assets/bp-analysis.png)

此外，您也需要設定CDN記錄轉送以進行流量分析。 LLM Optimizer需要代理和反向連結流量的Brand Presence資料和深入分析，以識別機會並提供規範性建議，協助客戶提升其AI可見度。

### 非AEM雲端客戶

在您簽署合約後，系統會透過slackbot命令將您加入，並附上您想要加入LLM Optimizer的網域。 此上線完成後，您將能夠透過[https://llmo.now](https://llmo.now)登入LLM Optimizer。

### 步驟2：自動預先填入深入分析

您的網域上線後，LLM Optimizer會自動填入以下內容：

* **類別** — 與您的網域相關的廣泛內容區域。
* **主題** — 與您的網域關聯的大量非品牌關鍵字相連結的特定主題。
* **提示** — 查詢（品牌和非品牌）以提供基線可見度。

這可確保在新增自訂設定和輸入之前，您就能看到品牌可見度的初始見解。

### 步驟3：自訂類別、主題和提示

按一下[客戶設定儀表板](/help/dashboards/customer-configuration.md)以開始自訂您的類別、主題和提示。

![客戶設定儀表板](/help/dashboards/assets/customer-config.png)

從此控制面板，您可以：

* 新增符合您業務優先順序的新類別。
* 輸入需要追蹤的自訂主題或子主題。
* 建立提示以監視特定查詢中的可見性。
* 定義提及別名，以便擷取所有提及。
* 定義競爭者別名以精確追蹤競爭者。

### 步驟4：提供CDN記錄轉送的資訊

若要解除鎖定代理流量和反向連結流量深入分析，您必須提供CDN記錄轉送的相關資訊。 如需如何設定記錄檔轉送的詳細資訊，請參閱每個特定頁面：

* [代理流量](/help/dashboards/agentic-traffic.md)
* [反向連結流量](/help/dashboards/referral-traffic.md#setup#cdn-setup)

### 步驟5：探索儀表板並採取行動

提供CDN記錄轉送的資訊後，您可以：

* 檢視[品牌存在](/help/dashboards/brand-presence.md)儀表板、檢視您的可見度分數，並追蹤您相對於競爭對手的績效。
* 探索[代理程式](/help/dashboards/agentic-traffic.md)和[轉介流量](/help/dashboards/referral-traffic.md)儀表板。
* 使用[機會](/help/dashboards/opportunities.md)來識別內容與技術改進。
* 匯出資料並與您的團隊共同作業，或邀請您的同事使用產品。

若要完全瞭解LLM最佳化程式的功能，請探索所有可用的[儀表板](/help/dashboards/dashboards-overview.md)。
