---
title: 新增LLM易記摘要
description: 瞭解LLM Optimizer如何識別缺少AI代理程式簡明摘要和關鍵點的高流量頁面，以及如何使用Edge最佳化檢閱和部署這些頁面。
feature: Opportunities
source-git-commit: 7f0729839d761ca57236da935c8c7638dd92f32a
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---


# 新增LLM易記摘要

新增LLM友善摘要機會可識別缺乏簡潔結構化摘要的高流量頁面，這使得AI代理程式難以快速瞭解頁面上的關鍵資訊。 它會以您現有的頁面內容為基礎，提供清楚的摘要和關鍵點。 這有助於代理程式更有效率地詮釋及擷取重要品牌宣告，並提高將您的內容準確包含在AI回應中的可能性。

您可以針對每個受影響的URL，檢閱AI產生的建議，然後透過[在Edge](/help/dashboards/optimize-at-edge/overview.md)最佳化來部署建議，讓代理流量變得更清晰、可快速瀏覽的內容，而不需要任何內容管理系統(CMS)變更。

## 如何修正問題

使用[在Edge](/help/dashboards/optimize-at-edge/overview.md)最佳化(Optimize)套用修正，其中：

- 向AI代理程式提供預先轉譯的HTML快照。
- 透過擷取的HTML中的摘要及/或關鍵點豐富頁面。
- 適用於CDN層（CMS不會變更）。
- 僅限AI — 不會對人類訪客或SEO機器人造成影響。
- 幾分鐘即可部署，而且可從LLM Optimizer介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer會識別頁面或區段層級&#x200B;**摘要**&#x200B;和&#x200B;**關鍵點**&#x200B;有助於AI理解的高流量頁面。 受影響的URL會出現在&#x200B;**目前的建議**&#x200B;標籤上含建議&#x200B;**的** URL中，您可以在此展開一列來檢查每個建議。

![URL包含目前建議的建議，展開列包含頁面和區段摘要建議](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-expand.png)

含有建議的&#x200B;**URL**&#x200B;表格列出摘要有助於代理程式探索的頁面。 建議會整理為&#x200B;**目前的建議**、**已修正的建議**&#x200B;和&#x200B;**已忽略的建議**。 對於每個URL，您可以：

- **展開列**&#x200B;以檢視分析和提議的摘要文字（以及包含時的要點）。
- **預覽**&#x200B;代理流量的比較前與比較後。
- 如果您在LLM Optimizer外部處理商機，請&#x200B;**標示為已修正**。
- **忽略不相關的**&#x200B;建議。

每個展開的專案會顯示頁面層級和區段層級的摘要指示、**AI產生的**&#x200B;復本、編輯控制項以及與即時頁面相連結的內容。

在&#x200B;**動作**&#x200B;資料行中按一下&#x200B;**預覽**&#x200B;以開啟最佳化預覽。 它會比較您的頁面在最佳化後檢視中，是否有一般流量（例如，插入與建議位置對齊的&#x200B;**摘要**&#x200B;和&#x200B;**關鍵點**&#x200B;內容）。 您可以在部署前隨時開啟或關閉該預覽。

當您準備好要發佈時，請使用核取方塊選取摘要和關鍵點條列專案。 頁尾顯示選取了多少項，並提供&#x200B;**標籤為已修正**、**忽略建議**&#x200B;和&#x200B;**部署最佳化**。

![目前建議，已選取摘要行專案，並在頁尾部署最佳化](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-url.png)

### 部署最佳化

當您準備在邊緣發佈時，請按一下&#x200B;**部署最佳化**。 **部署至Edge**&#x200B;對話方塊會列出選取的URL和最佳化詳細資料。 檢閱清單，然後選擇&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署至Edge對話方塊](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-dialog.png)

成功部署後，**部署完成**&#x200B;會確認有多少最佳化已上線，並注意AI代理程式可能需要一些時間索引更新。 關閉對話方塊並開啟&#x200B;**修正建議**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-confirm.png)

>[!NOTE]
>
>部署最佳化需要在Edge上線流程中完成最佳化。 如果您尚未上線，按一下&#x200B;**部署最佳化**&#x200B;會將您導向到上線流程。 如需Edge最佳化如何運作、支援的CDN提供者和上線流程的完整詳細資訊，請參閱[Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 修正建議和即時檢視

在&#x200B;**已修正的建議**&#x200B;上，部署的URL在狀態列中顯示&#x200B;**已最佳化**。 展開一列以檢閱已部署的摘要副本和指示。

![已修正建議索引標籤，其狀態為最佳化、已展開的部署摘要、檢視即時和詳細資料](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-fixed.png)

按一下資料列上的&#x200B;**即時檢視**&#x200B;以開啟作為驗證的&#x200B;**目前頁面內容**&#x200B;的唯讀檢視（包括插入的&#x200B;**摘要**&#x200B;和&#x200B;**套用的關鍵點**&#x200B;區塊）。 使用&#x200B;**詳細資料**&#x200B;進行分析。 當您需要大量回覆邊緣變更時，請使用核取方塊選取最佳化的列，然後在標題中使用&#x200B;**Rollback**。

![已修正建議，在回覆之前，包含大量選取的核取方塊](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-in-fixed.png)

## 復原

如果您改變心意，可以復原任何已部署的最佳化。 從&#x200B;**修正建議**&#x200B;檢視中，選取您要回覆的最佳化資料列，然後按一下標頭中的&#x200B;**回覆**。

**回覆**&#x200B;對話方塊會列出要回覆的建議，並附上簡短的警告，說明部署的最佳化將會回覆。 確認清單，然後按一下&#x200B;**回覆**&#x200B;或&#x200B;**取消**。

![復原對話方塊列出回復建議](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-dialog.png)

作業完成後，會顯示&#x200B;**已成功回覆**&#x200B;摘要；請關閉摘要以返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-confirm.png)

## 在示範中試用

探索[Frescopa示範](https://play.llmo.now/org/demo-org)中的「新增LLM易記摘要」工作流程。
