---
title: 新增適合 LLM 讀取的摘要
description: 了解 LLM Optimizer 如何識別缺少 AI 代理所需簡潔摘要與重點內容的高流量頁面，以及如何透過邊緣最佳化進行審閱與部署。
feature: Opportunities
autotag-review: '2026-05-15T17:27:51.631Z'
TQID: 'https://experienceleague.adobe.com/QpBdx3B-qg41ZWtPU2R4CNq-POrSs31UIb0kms1H3GU'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 793
ht-degree: 100%

---


# 新增適合 LLM 讀取的摘要

「新增適合 LLM 讀取的摘要」機會可識別缺乏簡潔結構化摘要，致使 AI 代理難以快速理解頁面中關鍵資訊的高流量頁面。 此機會根據您現有的頁面內容，提供清楚的摘要與重點內容。 這有助於 AI 代理更有效率地解讀與擷取重要的品牌聲明，並提高您的內容被準確納入 AI 回答中的可能性。

針對每個受影響的 URL，您可以審閱 AI 生成的建議，然後透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)部署建議，讓代理式流量取得更清晰、易於瀏覽的背景資訊，且無需變更內容管理系統 (CMS)。

## 其修正問題的方式

將透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)套用修正，且具備以下特點：

- 為 AI 代理提供預先轉譯的 HTML 快照。
- 透過 AI 代理擷取的 HTML 中的摘要和/或重點內容，擴充頁面內容。
- 在內容傳遞網路層運作 (無需變更 CMS)。
- 僅針對 AI 運作，不會影響真人訪客或搜尋引擎最佳化機器人。
- 只需幾分鐘即完成部署，且可透過 LLM Optimizer 介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer 會識別在頁面或區段層級加入&#x200B;**摘要**&#x200B;和&#x200B;**重點內容**&#x200B;可協助 AI 理解的高流量頁面。 受影響的 URL 會出現在&#x200B;**「目前建議」**&#x200B;標籤的&#x200B;**「有建議項目的 URL」**&#x200B;表格中，而您可以顯示資料列來檢視各個建議。

![目前建議中的有建議項目的 URL，已顯示資料列並有頁面與區段摘要建議。](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-expand.png)

**「有建議項目的 URL」**&#x200B;表格會列出加入摘要可提升代理式搜尋能力的頁面。 建議項目分為&#x200B;**「目前建議」**、**「已修正建議」**&#x200B;和&#x200B;**「已忽略建議」**。 針對每個 URL，您可以：

- **顯示資料列**&#x200B;以檢視分析和建議的摘要文字 (以及重點內容，若有包含)。
- **預覽**&#x200B;代理式流量變更前後的比較。
- **標記為已修正**，如果您是在 LLM Optimizer 之外處理機會。
- **忽略**&#x200B;不相關的建議。

每個顯示的項目都會顯示頁面層級與區段層級的摘要指示、**AI 生成的**&#x200B;副本、編輯控制項以及與即時頁面相關的背景資訊。

按一下&#x200B;**「動作」**&#x200B;欄位的&#x200B;**「預覽」**&#x200B;即可開啟最佳化預覽。 此功能可比較目前代理式流量看到的頁面，以及最佳化後的頁面視圖 (例如，插入與建議位置對齊的&#x200B;**摘要**&#x200B;和&#x200B;**重點內容**)。 您可以在部署前隨時開啟或關閉該預覽。

當您準備好要發佈時，請使用核取方塊選取摘要與重點內容項目。 頁尾會顯示已選取項目的數量，並提供&#x200B;**「標記為已修正」**、**「忽略建議」**&#x200B;和&#x200B;**「部署最佳化項目」**。

![目前建議和已選取的摘要項目，以及頁尾的部署最佳化項目](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-url.png)

### 部署最佳化項目

當您準備好發佈至邊緣時，請按一下&#x200B;**「部署最佳化項目」**。 **「部署至邊緣」**&#x200B;對話框會列出選取的 URL 和最佳化詳細資訊。 審閱清單，然後選擇&#x200B;**「部署」**&#x200B;或&#x200B;**「取消」**。

![部署至邊緣對話框](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-dialog.png)

成功部署後，**「部署完成」**&#x200B;會確認有多少最佳化項目已上線，並提醒 AI 代理可能需要一些時間才能索引更新的內容。 關閉對話框並開啟&#x200B;**「已修正建議」**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-confirm.png)

>[!NOTE]
>
>部署最佳化項目之前，必須先完成邊緣最佳化的上線流程。 如果您尚未上線，按一下&#x200B;**「部署最佳化項目」**，系統便會將您導向上線流程。 如需了解邊緣最佳化的運作方式、支援的內容傳遞網路提供者和上線流程的完整詳細資訊，請參閱[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 已修正建議和檢視即時版本

在&#x200B;**「已修正建議」**&#x200B;中，已部署的 URL 會在狀態欄顯示&#x200B;**「已最佳化」**。 顯示資料列以審閱已部署的摘要內容和指示。

![已修正建議分頁標籤和已最佳化狀態、已顯示的部署摘要，以及檢視即時版本與詳細資訊](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-fixed.png)

按一下資料列中的&#x200B;**「檢視即時版本」**，以唯讀模式開啟為進行檢驗而提供的&#x200B;**目前頁面內容** (包括已套用的&#x200B;**摘要**&#x200B;和&#x200B;**重點內容**&#x200B;區塊)。 使用&#x200B;**「詳細資訊」**&#x200B;進行分析。 當您需要大量復原邊緣變更時，請使用核取方塊選取已最佳化的資料列，然後使用標頭中的&#x200B;**「復原」**。

![已修正建議，以及在復原之前的大量選取核取方塊](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-in-fixed.png)

## 復原

如果您改變主意，可以復原任何已部署的最佳化項目。 在&#x200B;**「已修正建議」**&#x200B;視圖中，選取您要還原的已最佳化資料列，然後按一下標頭中的&#x200B;**「復原」**。

**「復原」**&#x200B;對話框會列出將要復原的建議，並附上簡短的警告，指出已部署的最佳化項目將會還原。 確認清單，然後按一下&#x200B;**「復原」**&#x200B;或&#x200B;**「取消」**。

![復原對話框列出將還原的建議](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-dialog.png)

操作完成後將會顯示&#x200B;**「已成功復原」**&#x200B;摘要；關閉摘要即可返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-confirm.png)

## 在示範中試用

探索 [Frescopa 示範](https://play.llmo.now/org/demo-org)中的「新增適合 LLM 讀取的摘要」工作流程。
