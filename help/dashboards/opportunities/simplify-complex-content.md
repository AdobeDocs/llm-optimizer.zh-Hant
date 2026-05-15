---
title: 簡化複雜的內容
description: 瞭解LLM Optimizer如何使用高流量頁面識別AI代理程式難以解讀的密集副本，以及如何使用Edge最佳化檢閱和部署簡化文字。
feature: Opportunities
autotag-review: '2026-05-15T17:58:39.879Z'
TQID: 'https://experienceleague.adobe.com/wO3ZY-fEgOi7cD4dq0kCltk-YJSD431bkA6-9PW42Lo'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 785
ht-degree: 1%

---


# 簡化複雜的內容

簡化複雜內容機會可識別高流量頁面，其中密集或複雜文字會讓AI代理程式難以解譯關鍵資訊。 它引入了更清晰、更易於掃描的現有副本版本，同時保留了原始含義。 這有助於代理程式更可靠地剖析、摘要及擷取重要資訊。

對於每個受影響的URL，您可以檢閱&#x200B;**已改善文字**&#x200B;建議、將其與&#x200B;**預覽**&#x200B;進行比較，然後透過[在Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)來部署這些建議，如此一來，代理式流量接收更清楚的HTML，而不需要任何內容管理系統(CMS)變更。

## 如何修正問題

使用[在Edge](/help/dashboards/optimize-at-edge/overview.md)最佳化(Optimize)套用修正，其中：

- 向AI代理程式提供預先轉譯的HTML快照。
- 更新代理程式可見頁面，以取代複雜段落，或以&#x200B;**改善文字**&#x200B;與即時頁面對齊。
- 適用於CDN層（CMS不會變更）。
- 僅限AI — 不會對人類訪客或SEO機器人造成影響。
- 幾分鐘即可部署，而且可從LLM Optimizer介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer會識別接收高代理流量且內容分數低於可讀性臨界值的頁面，然後建議重寫副本。 對於每個頁面，您有：

**改善文字** — 簡化內容以頁面上已有的內容為基礎。
**預覽** — 代理流量的比較前與比較後。

受影響的URL會出現在&#x200B;**目前的建議**&#x200B;標籤上含建議&#x200B;**的** URL中，您可以在此展開一列來檢查每個建議。

![個URL，包含目前建議的建議、擴充的列，以及改善的文字和預覽](/help/dashboards/opportunities/assets/simplify-complex-content-expand.png)

含有建議的&#x200B;**URL**&#x200B;表格列出簡化內容有助於全面理解的頁面。 建議會整理為&#x200B;**目前的建議**、**已修正的建議**&#x200B;和&#x200B;**已忽略的建議**。 對於每個URL，您可以：

- **展開列**&#x200B;以檢視該頁面的&#x200B;**改善文字**&#x200B;建議。
- **預覽**&#x200B;代理流量的比較前與比較後。
- 如果您在LLM Optimizer外部處理商機，請&#x200B;**標示為已修正**。
- **忽略不相關的**&#x200B;建議。

**檢視**&#x200B;包含&#x200B;**目前的建議**、**已修正的建議** (狀態&#x200B;**已最佳化** （部署時）)以及&#x200B;**已忽略的建議**。 您可以在&#x200B;**已修正的建議**&#x200B;上使用&#x200B;**檢視即時**&#x200B;來驗證即時部署，並隨時回覆。

使用核取方塊選取您想要出貨的URL或含有&#x200B;**已改善文字**&#x200B;的行專案，然後使用&#x200B;**標籤為已修正**、**忽略建議**&#x200B;或&#x200B;**在**&#x200B;機會計畫&#x200B;**標題中部署最佳化**。 示範UI也會顯示選取專案計數以及清單中的相關動作。

![計畫標題中的機會計畫、目前的建議、展開列和部署最佳化](/help/dashboards/opportunities/assets/simplify-complex-content-select.png)

### 部署最佳化

當您準備在邊緣發佈時，請按一下&#x200B;**部署最佳化**。 **部署至Edge**&#x200B;對話方塊會列出選取的URL和最佳化詳細資料。 檢閱清單，然後選擇&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署至Edge對話方塊](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-dialog.png)

成功部署後，**部署完成**&#x200B;會確認有多少最佳化已上線，並注意AI代理程式可能需要一些時間索引更新。 關閉對話方塊並開啟&#x200B;**修正建議**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-confirm.png)

>[!NOTE]
>
>部署最佳化需要在Edge上線流程中完成最佳化。 如果您尚未上線，按一下&#x200B;**部署最佳化**&#x200B;會將您導向到上線流程。 如需Edge最佳化如何運作、支援的CDN提供者和上線流程的完整詳細資訊，請參閱[Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 修正建議和即時檢視

在&#x200B;**已修正的建議**&#x200B;上，部署的URL在狀態列中顯示&#x200B;**已最佳化**。 展開一列以檢閱已部署的&#x200B;**已改善文字**&#x200B;和指示。

![已修正建議索引標籤，其狀態為「已最佳化」、已展開的簡化副本、已檢視即時和詳細資料](/help/dashboards/opportunities/assets/simplify-complex-content-fixed.png)

按一下資料列上的&#x200B;**即時檢視**，開啟作為驗證的&#x200B;**目前頁面內容**&#x200B;的唯讀檢視（包括套用的簡化段落）。 使用&#x200B;**詳細資料**&#x200B;進行分析。

![即時檢視 — 目前的頁面內容，包括代理程式的簡化文字](/help/dashboards/opportunities/assets/simplify-complex-content-view-live.png)

當您需要大量回覆邊緣變更時，請使用核取方塊選取最佳化的列，然後在標題中使用&#x200B;**Rollback**。

![已修正建議，在標題](/help/dashboards/opportunities/assets/simplify-complex-content-rollback.png)中展開已部署的列、最佳化狀態和回覆

## 復原

如果您改變心意，可以復原任何已部署的最佳化。 從&#x200B;**修正建議**&#x200B;檢視中，選取您要回覆的最佳化資料列，然後按一下標頭中的&#x200B;**回覆**。

**回覆**&#x200B;對話方塊會列出要回覆的建議，並附上簡短的警告，說明部署的最佳化將會回覆。 確認清單，然後按一下&#x200B;**回覆**&#x200B;或&#x200B;**取消**。

![復原對話方塊列出回復建議](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-dialog.png)

作業完成後，會顯示&#x200B;**已成功回覆**&#x200B;摘要；請關閉摘要以返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-confirm.png)

## 在示範中試用

在[Frescopa示範](https://play.llmo.now/org/demo-org)中探索簡化複雜內容的工作流程。
