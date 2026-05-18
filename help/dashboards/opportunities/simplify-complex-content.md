---
title: 簡化複雜的內容
description: 了解 LLM Optimizer 如何識別內容過於密集而導致 AI 代理難以解讀的高流量頁面，以及如何透過邊緣最佳化審閱並部署簡化文字。
feature: Opportunities
autotag-review: '2026-05-15T17:58:39.879Z'
TQID: 'https://experienceleague.adobe.com/wO3ZY-fEgOi7cD4dq0kCltk-YJSD431bkA6-9PW42Lo'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 785
ht-degree: 100%

---


# 簡化複雜的內容

「簡化複雜的內容」機會可識別內容過於密集或複雜，導致 AI 代理難以解讀關鍵資訊的高流量頁面。 此機會可在保留現有內容原始意義的同時，提供更清晰且更易於瀏覽的版本。 這能協助 AI 代理更可靠地剖析、摘要及擷取重要資訊。

針對每個受影響的 URL，您可以審閱&#x200B;**「改良文本」**&#x200B;建議，並與&#x200B;**「預覽」**&#x200B;進行比較，然後透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)部署這些建議，讓代理式流量取得更清晰的 HTML，且無需變更內容管理系統 (CMS)。

## 其修正問題的方式

將透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)套用修正，且具備以下特點：

- 為 AI 代理提供預先轉譯的 HTML 快照。
- 更新 AI 代理可見頁面，利用與即時頁面相符的&#x200B;**「改良文本」**&#x200B;取代或補充複雜的段落。
- 在內容傳遞網路層運作 (無需變更 CMS)。
- 僅針對 AI 運作，不會影響真人訪客或搜尋引擎最佳化機器人。
- 只需幾分鐘即完成部署，且可透過 LLM Optimizer 介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer 會識別接收到大量代理式流量以及內容分數低於可讀性門檻的頁面，然後提出內容改寫建議。 每個頁面皆有以下項目：

**改良文本** - 以頁面現有內容為基礎的簡化內容。
**預覽** - 代理式流量變更前後的比較。

受影響的 URL 會出現在&#x200B;**「目前建議」**&#x200B;標籤的&#x200B;**「有建議項目的 URL」**&#x200B;表格中，而您可以顯示資料列來檢視各個建議。

![目前建議上的有建議項目的 URL，顯示的資料列含改良文本與預覽](/help/dashboards/opportunities/assets/simplify-complex-content-expand.png)

**「有建議項目的 URL」**&#x200B;表格會列出簡化內容有助於 AI 代理理解的頁面。 建議項目分為&#x200B;**「目前建議」**、**「已修正建議」**&#x200B;和&#x200B;**「已忽略建議」**。 針對每個 URL，您可以：

- **顯示資料列**&#x200B;以檢視該頁面的&#x200B;**「改良文本」**&#x200B;建議。
- **預覽**&#x200B;代理式流量變更前後的比較。
- **標記為已修正**，如果您是在 LLM Optimizer 之外處理機會。
- **忽略**&#x200B;不相關的建議。

各個&#x200B;**視圖**&#x200B;包含&#x200B;**「目前建議」**、**「已修正建議」** (部署後狀態為&#x200B;**「已最佳化」**) 以及&#x200B;**「已忽略建議」**。 您可以在&#x200B;**「已修正建議」**&#x200B;中使用&#x200B;**「檢視即時版本」**&#x200B;來驗證即時部署，並隨時復原。

使用核取方塊選取您要發佈的 URL 或含有&#x200B;**「改良文本」**&#x200B;的條列項目，然後在&#x200B;**「機會計劃」**&#x200B;標頭中使用&#x200B;**「標記為已修正」**、**「忽略建議」**&#x200B;或&#x200B;**「部署最佳化項目」**。 示範 UI 也會顯示選取項目數量，以及與清單相關的動作。

![計劃標頭中的機會計劃、目前建議、顯示的資料列和部署最佳化項目](/help/dashboards/opportunities/assets/simplify-complex-content-select.png)

### 部署最佳化項目

當您準備好發佈至邊緣時，請按一下&#x200B;**「部署最佳化項目」**。 **「部署至邊緣」**&#x200B;對話框會列出選取的 URL 和最佳化詳細資訊。 審閱清單，然後選擇&#x200B;**「部署」**&#x200B;或&#x200B;**「取消」**。

![部署至邊緣對話框](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-dialog.png)

成功部署後，**「部署完成」**&#x200B;會確認有多少最佳化項目已上線，並提醒 AI 代理可能需要一些時間才能索引更新的內容。 關閉對話框並開啟&#x200B;**「已修正建議」**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-confirm.png)

>[!NOTE]
>
>部署最佳化項目之前，必須先完成邊緣最佳化的上線流程。 如果您尚未上線，按一下&#x200B;**「部署最佳化項目」**，系統便會將您導向上線流程。 如需了解邊緣最佳化的運作方式、支援的內容傳遞網路提供者和上線流程的完整詳細資訊，請參閱[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 已修正建議和檢視即時版本

在&#x200B;**「已修正建議」**&#x200B;中，已部署的 URL 會在狀態欄顯示&#x200B;**「已最佳化」**。 顯示資料列，審閱已部署的&#x200B;**「改良文本」**&#x200B;和指示。

![已修正建議分頁標籤顯示已最佳化狀態，顯示的簡化內容、檢視即時版本及詳細資訊](/help/dashboards/opportunities/assets/simplify-complex-content-fixed.png)

按一下資料列上的&#x200B;**「檢視即時版本」**，以唯讀模式開啟為進行驗證而提供的&#x200B;**目前頁面內容** (包括套用的簡化段落)。 使用&#x200B;**「詳細資訊」**&#x200B;進行分析。

![檢視即時版本：包含供 AI 代理使用的簡化文字的目前頁面內容](/help/dashboards/opportunities/assets/simplify-complex-content-view-live.png)

當您需要大量復原邊緣變更時，請使用核取方塊選取已最佳化的資料列，然後使用標頭中的&#x200B;**「復原」**。

![已修正建議，顯示已部署的資料列、已最佳化狀態以及標頭中的復原](/help/dashboards/opportunities/assets/simplify-complex-content-rollback.png)

## 復原

如果您改變主意，可以復原任何已部署的最佳化項目。 在&#x200B;**「已修正建議」**&#x200B;視圖中，選取您要還原的已最佳化資料列，然後按一下標頭中的&#x200B;**「復原」**。

**「復原」**&#x200B;對話框會列出將要復原的建議，並附上簡短的警告，指出已部署的最佳化項目將會還原。 確認清單，然後按一下&#x200B;**「復原」**&#x200B;或&#x200B;**「取消」**。

![復原對話框列出將還原的建議](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-dialog.png)

操作完成後將會顯示&#x200B;**「已成功復原」**&#x200B;摘要；關閉摘要即可返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-confirm.png)

## 在示範中試用

在 [Frescopa 示範](https://play.llmo.now/org/demo-org)中探索「簡化複雜內容」工作流程。
