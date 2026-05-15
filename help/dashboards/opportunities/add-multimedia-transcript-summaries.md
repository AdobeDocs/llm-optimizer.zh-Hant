---
title: 新增多媒體成績單摘要
description: 瞭解LLM Optimizer如何在沒有機器可讀文字的視訊中識別關鍵資訊嵌入的頁面，以及如何透過Edge的「最佳化」檢閱和部署AI產生的成績單摘要。
feature: Opportunities
autotag-review: '2026-05-15T17:28:28.569Z'
TQID: 'https://experienceleague.adobe.com/LiXMsMq6D08ciXR85aQBNDpmR5Csiv-5b9kv3lfTpDc'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 775
ht-degree: 0%

---


# 新增多媒體成績單摘要

>[!NOTE]
>
> **提早存取** — 新增多媒體成績單摘要可供提早存取。 隨著功能日漸成熟，工作流程的可用性、資格和部分可能會有所變更。 如果您對存取權有任何疑問，請聯絡您的Adobe帳戶團隊。

「新增多媒體筆錄摘要」機會可識別重要資訊存在於視訊或其他媒體中的頁面，這些頁面沒有筆錄或AI人員可閱讀的簡短文字摘要。 它會以媒體和周圍的頁面內容為基礎，介紹&#x200B;**AI產生的成績單摘要**。 它可讓AI代理程式瞭解多媒體內容，藉此協助復原原本會遺漏的重要品牌資訊。

對於每個受影響的URL，您可以檢閱建議的&#x200B;**內容修補程式**、**實作**&#x200B;詳細資料（例如，目標CSS選取器和作業）和&#x200B;**基本原則**，然後使用[在Edge](/help/dashboards/optimize-at-edge/overview.md)最佳化(Optimize)進行部署，讓代理流量接收擴充的HTML，而不需要任何內容管理系統(CMS)變更。

## 如何修正問題

使用[在Edge](/help/dashboards/optimize-at-edge/overview.md)最佳化(Optimize)套用修正，其中：

- 向AI代理程式提供預先轉譯的HTML快照。
- 透過擷取的HTML中的成績單摘要文字豐富頁面（例如，接近相關內嵌視訊）。
- 適用於CDN層（CMS不會變更）。
- 僅限AI — 不會對人類訪客或SEO機器人造成影響。
- 幾分鐘即可部署，而且可從LLM Optimizer介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer會根據您的設定和頁面結構，標示內嵌媒體缺少機器可讀文字的高流量頁面。 受影響的URL會出現在&#x200B;**目前的建議**&#x200B;標籤上含建議的&#x200B;**URL**&#x200B;表格中，您可以在此展開一列來檢查每個&#x200B;**內容修補程式**、其套用方式，以及建議使用的原因。

對於每個頁面，您有：

**多媒體摘要** — 衍生自視訊內容的結構化摘要。
**預覽** — 頁面比較之前和之後。

![個URL，內含目前建議的建議、擴充的資料列（包含內容修補程式、實作詳細資料和理由）](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-expand.png)

含有建議的&#x200B;**URL**&#x200B;表格列出文字記錄或摘要文字有助於代理程式探索的頁面。 建議會整理為&#x200B;**目前的建議**、**已修正的建議**&#x200B;和&#x200B;**已忽略的建議**。 對於每個URL，您可以：

- **展開資料列**&#x200B;以檢視&#x200B;**內容修補程式**&#x200B;文字、**實作**&#x200B;詳細資料（包括計畫的DOM作業和CSS選取器）以及變更的&#x200B;**理由**。
- **預覽**&#x200B;代理流量的比較前與比較後。
- 如果您在LLM Optimizer外部處理商機，請&#x200B;**標示為已修正**。
- **忽略不相關的**&#x200B;建議。

如果支援（鉛筆控制項），您可以從列編輯修補程式文字，然後使用列核取方塊來選取要建置的專案。 頁尾顯示選取了多少項，並提供&#x200B;**標籤為已修正**、**忽略建議**&#x200B;和&#x200B;**部署最佳化**。

### 部署最佳化

當您準備在邊緣發佈時，請按一下&#x200B;**部署最佳化**。 **部署至Edge**&#x200B;對話方塊會列出您即將執行的URL、選取器和作業。 檢閱清單，然後選擇&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署到Edge對話方塊以取得多媒體文字記錄摘要內容修補程式](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-dialog.png)

成功部署後，**部署完成**&#x200B;會確認有多少最佳化已上線。 關閉對話方塊並開啟&#x200B;**修正建議**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-confirm.png)

>[!NOTE]
>
> 部署最佳化需要在Edge上線流程中完成最佳化。 如果您尚未上線，按一下&#x200B;**部署最佳化**&#x200B;會將您導向到上線流程。 如需Edge最佳化如何運作、支援的CDN提供者和上線流程的完整詳細資訊，請參閱[Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 修正建議和即時檢視

在&#x200B;**已修正的建議**&#x200B;上，部署的URL在狀態列中顯示&#x200B;**已最佳化**。 展開資料列以檢閱即時&#x200B;**內容修補程式**、**實作**&#x200B;詳細資料和&#x200B;**基本原則**。 此外，您可以將&#x200B;**詳細資料**&#x200B;用於分析，或將&#x200B;**即時檢視**&#x200B;用於確認代理流量會收到哪些內容。

![已修正最佳化狀態、展開的內容修補程式和復原的建議](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-fixed.png)

若要大量回覆，請使用核取方塊選取最佳化的列，然後在標題中使用&#x200B;**回覆**。

![修正了在回覆前選取資料列的建議](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-select-in-fixed.png)

## 復原

如果您改變心意，可以復原已部署的最佳化。 從&#x200B;**修正建議**，選取要還原的資料列，然後按一下&#x200B;**回覆**。

**復原**&#x200B;對話方塊列出將復原的建議，並警告部署的最佳化將從代理流量的即時路徑中移除。 確認，然後按一下&#x200B;**回覆**&#x200B;或&#x200B;**取消**。

![復原對話方塊列出回復建議](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-dialog.png)

作業完成後，會顯示&#x200B;**已成功回覆**&#x200B;摘要；請關閉摘要以返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-confirm.png)

## 在示範中試用

探索[Frescopa示範](https://play.llmo.now/org/demo-org)中的「新增多媒體成績單摘要」工作流程。
