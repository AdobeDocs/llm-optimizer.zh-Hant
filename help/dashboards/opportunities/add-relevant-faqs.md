---
title: 新增相關常見問題集
description: 了解 LLM Optimizer 如何識別缺少 AI 代理所需結構化常見問題集內容的高流量頁面，以及如何透過邊緣最佳化審閱與部署 AI 生成的常見問題集。
feature: Opportunities
autotag-review: '2026-07-15T16:47:24.291Z'
TQID: 'https://experienceleague.adobe.com/ObmJKEvR9-ovzugCtAsRkcUBemcsMw6cNwizkuKYPcc'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3a
subfeature_v2: id: bbfc1b77-44c5-4fe8-b65f-ec160fe0d021
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 742
ht-degree: 100%

---


# 新增相關常見問題集

「新增相關常見問題集」機會可識別缺少結構化常見問題集內容的高流量頁面，而 AI 代理在生成回答時通常會依賴這類內容。 此功能會根據現有的頁面資料，產生相關且&#x200B;**符合意圖的常見問題集**&#x200B;內容。 這有助於 AI 代理更直接地將使用者問題對應至您的內容。

對於每個受影響的 URL，您可以審閱 AI 生成的常見問題集建議，然後透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)部署這些內容，讓代理式流量接收到更清楚的常見問題集背景資訊，且無需變更內容管理系統 (CMS)。

## 其修正問題的方式

將透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)套用修正，且具備以下特點：

- 為 AI 代理提供預先轉譯的 HTML 快照。
- 在 AI 代理擷取的 HTML 中加入常見問題集內容，以擴充頁面資訊。
- 在內容傳遞網路層運作 (無需變更 CMS)。
- 僅針對 AI 運作，不會影響真人訪客或搜尋引擎最佳化機器人。
- 只需幾分鐘即完成部署，且可透過 LLM Optimizer 介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer 會根據您品牌的提示集，識別缺少常見問題集內容或內容不足的高流量頁面。 受影響的 URL 會出現在&#x200B;**「目前建議」**&#x200B;分頁標籤的&#x200B;**「有建議項目的 URL」**&#x200B;表格中，而您可以顯示資料列來檢視各個建議。

![目前建議上的有建議項目的 URL，已顯示的資料列包含常見問題集提示與 AI 生成回答](/help/dashboards/opportunities/assets/add-relevant-faqs-expand.png)

**「有建議項目的 URL」**&#x200B;表格列出常見問題集可協助 AI 驅動搜尋的頁面。 建議項目分為&#x200B;**「目前建議」**、**「已修正建議」**&#x200B;和&#x200B;**「已忽略建議」**。 針對每個 URL，您可以：

- **顯示資料列**&#x200B;以檢視該頁面的建議常見問題集內容。
- **預覽**&#x200B;代理式流量變更前後的比較。
- **標記為已修正**，如果您是在 LLM Optimizer 之外處理機會。
- **忽略**&#x200B;不相關的建議。

每個顯示的項目都會列出與頁面相關的常見問題集&#x200B;**提示**、**AI 生成的**&#x200B;建議回答、簡述&#x200B;**理由**&#x200B;和&#x200B;**資料來源**。 此表格也會顯示每個 URL 的常見問題建議數量，並有&#x200B;**「代理式流量 (4 週)」**&#x200B;協助您排定優先順序。

在資料列按一下&#x200B;**「預覽」**&#x200B;以開啟最佳化項目預覽。 此功能會比較您的頁面目前對代理式流量顯示的內容以及最佳化後的視圖 (例如，新的&#x200B;**常見問題集**&#x200B;區塊)。

![最佳化項目預覽，比較目前以及具備常見問題集之最佳化後 AI 代理視圖](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-01.png)

請使用每列的核取方塊，選取您要正式推出的常見問題建議。 頁尾會顯示已選取項目的數量，並提供&#x200B;**「標記為已修正」**、**「忽略建議」**&#x200B;和&#x200B;**「部署最佳化項目」**。

![已選取「目前建議」中的常見問題建議以及「部署最佳化項目」](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-02.png)

### 部署最佳化項目

當您準備好發佈至邊緣時，請按一下&#x200B;**「部署最佳化項目」**。 **「部署至邊緣」**&#x200B;對話框會列出您即將推播的 URL、問題和回答。 審閱清單，然後選擇&#x200B;**「部署」**&#x200B;或&#x200B;**「取消」**。

![部署至邊緣對話框](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-03.png)

成功部署後，**「部署完成」**&#x200B;會確認有多少最佳化項目已上線。 關閉對話框並開啟&#x200B;**「已修正建議」**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-04.png)

>[!NOTE]
>
>部署最佳化項目之前，必須先完成邊緣最佳化的上線流程。 如果您尚未上線，按一下&#x200B;**「部署最佳化項目」**，系統便會將您導向上線流程。 如需了解邊緣最佳化的運作方式、支援的內容傳遞網路提供者和上線流程的完整詳細資訊，請參閱[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 已修正建議和檢視即時版本

在&#x200B;**「已修正建議」**&#x200B;中，已部署的 URL 會在狀態欄顯示&#x200B;**「已最佳化」**。 顯示資料列以審閱即時的常見問題集內容，使用&#x200B;**「詳細資訊」**&#x200B;查看分析，或按一下&#x200B;**「檢視即時版本」**&#x200B;以唯讀模式開啟為進行驗證而提供的&#x200B;**目前頁面內容** (包括插入的&#x200B;**常見問題集**&#x200B;區段)。

![已修正建議及已最佳化狀態、檢視即時版本和復原](/help/dashboards/opportunities/assets/add-relevant-faqs-fixed.png)

**「檢視即時版本」**&#x200B;視窗會顯示該檢查中顯示的頁面結構和常見問題集內容。

![檢視即時版本 — 包含常見問題集的目前頁面內容](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-05.png)

## 復原

如果您改變主意，可以復原任何已部署的最佳化項目。 在&#x200B;**「已修正建議」**&#x200B;視圖中，您可以選取要還原的最佳化資料列，然後按一下標頭中的&#x200B;**「復原」**。

**「復原」**&#x200B;對話框會列出將要復原的建議，並附上簡短的警告，指出已部署的最佳化項目將會還原。 確認清單，然後按一下&#x200B;**「復原」**&#x200B;或&#x200B;**「取消」**。

![復原對話框列出將還原的建議](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-07.png)

操作完成後將會顯示&#x200B;**「已成功復原」**&#x200B;摘要；關閉摘要即可返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-08.png)

## 在示範中試用

探索 [Frescopa 操作示範](https://play.llmo.now/org/demo-org)中「新增相關常見問題集」工作流程。
