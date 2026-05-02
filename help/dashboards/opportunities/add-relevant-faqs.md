---
title: 新增相關常見問答集
description: 瞭解LLM Optimizer如何識別缺少AI代理程式結構化常見問答內容的高流量頁面，以及如何在Edge使用「最佳化」檢閱和部署AI產生的常見問答。
feature: Opportunities
source-git-commit: 7f0729839d761ca57236da935c8c7638dd92f32a
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 1%

---


# 新增相關常見問答集

「新增相關常見問題集」機會可識別缺少結構化問答內容的高流量頁面，AI代理在產生回應時通常依賴這些內容。 它以您現有的頁面資料為基礎，推出相關的&#x200B;**意圖一致的常見問題集**&#x200B;內容。 這可協助代理更直接地將使用者問題與您的內容比對。

對於每個受影響的URL，您可以檢閱AI產生的常見問題集建議，然後透過[在Edge](/help/dashboards/optimize-at-edge/overview.md)最佳化來部署這些建議，如此一來，代理流量就能收到更清楚的問答內容，而不需要任何內容管理系統(CMS)變更。

## 如何修正問題

使用[在Edge](/help/dashboards/optimize-at-edge/overview.md)最佳化(Optimize)套用修正，其中：

- 向AI代理程式提供預先轉譯的HTML快照。
- 透過擷取的HTML中的常見問題集內容豐富頁面。
- 適用於CDN層（CMS不會變更）。
- 僅限AI — 不會對人類訪客或SEO機器人造成影響。
- 幾分鐘即可部署，而且可從LLM Optimizer介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer會根據您品牌的提示集，識別缺少問答內容或內容較少的高流量頁面。 受影響的URL會出現在&#x200B;**目前的建議**&#x200B;標籤上含建議&#x200B;**的** URL中，您可以在此展開一列來檢查每個建議。

![URL包含目前建議的建議、展開列包含FAQ提示和AI產生的回答](/help/dashboards/opportunities/assets/add-relevant-faqs-expand.png)

含有建議的&#x200B;**URL**&#x200B;表格列出常見問題集可協助AI驅動探索的頁面。 建議會整理為&#x200B;**目前的建議**、**已修正的建議**&#x200B;和&#x200B;**已忽略的建議**。 對於每個URL，您可以：

- **展開列**&#x200B;以檢視該頁面的建議常見問題集內容。
- **預覽**&#x200B;代理流量的比較前與比較後。
- 如果您在LLM Optimizer外部處理商機，請&#x200B;**標示為已修正**。
- **忽略不相關的**&#x200B;建議。

每個展開的專案都列出與頁面相連結的FAQ **提示**、**AI產生的**&#x200B;建議答案、簡短&#x200B;**推理**&#x200B;和&#x200B;**來源**。 此表格也顯示每個URL和&#x200B;**代理流量（4週）**&#x200B;建議的常見問題集數目，以協助您排定優先順序。

按一下資料列上的&#x200B;**預覽**&#x200B;以開啟最佳化預覽。 它會比較您的頁面現在對一般流量的外觀與最佳化後檢視（例如，新的&#x200B;**常見問題**&#x200B;區塊）。

![比較目前與最佳化後代理程式檢視的預覽最佳化（有常見問答）](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-01.png)

使用列核取方塊選取您要出貨的常見問題集建議。 頁尾顯示選取了多少項，並提供&#x200B;**標籤為已修正**、**忽略建議**&#x200B;和&#x200B;**部署最佳化**。

![已選取關於部署最佳化之目前建議的常見問題集建議](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-02.png)

### 部署最佳化

當您準備在邊緣發佈時，請按一下&#x200B;**部署最佳化**。 **部署至Edge**&#x200B;對話方塊會列出您即將推送的URL、問題和答案。 檢閱清單，然後選擇&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署至Edge對話方塊](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-03.png)

成功部署後，**部署完成**&#x200B;會確認有多少最佳化已上線。 關閉對話方塊並開啟&#x200B;**修正建議**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-04.png)

>[!NOTE]
>
>部署最佳化需要在Edge上線流程中完成最佳化。 如果您尚未上線，按一下&#x200B;**部署最佳化**&#x200B;會將您導向到上線流程。 如需Edge最佳化如何運作、支援的CDN提供者和上線流程的完整詳細資訊，請參閱[Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 修正建議和即時檢視

在&#x200B;**已修正的建議**&#x200B;上，部署的URL在狀態列中顯示&#x200B;**已最佳化**。 展開一列以檢閱即時常見問答集內容，使用&#x200B;**詳細資料**&#x200B;進行分析，或按一下&#x200B;**檢視即時**&#x200B;以開啟作為驗證的&#x200B;**目前頁面內容**&#x200B;的唯讀檢視（包括插入的&#x200B;**常見問答集**&#x200B;區段）。

![已修正最佳化狀態、檢視即時和復原的建議](/help/dashboards/opportunities/assets/add-relevant-faqs-fixed.png)

**檢視即時**&#x200B;視窗會顯示該檢查中顯示的頁面結構和常見問答集副本。

![即時檢視 — 目前頁面內容包含常見問答集](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-05.png)

## 復原

如果您改變心意，可以復原任何已部署的最佳化。 從&#x200B;**修正建議**&#x200B;檢視中，您可以選取要還原的最佳化資料列，然後按一下標頭中的&#x200B;**回覆**。

**回覆**&#x200B;對話方塊會列出要回覆的建議，並附上簡短的警告，說明部署的最佳化將會回覆。 確認清單，然後按一下&#x200B;**回覆**&#x200B;或&#x200B;**取消**。

![復原對話方塊列出回復建議](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-07.png)

作業完成後，會顯示&#x200B;**已成功回覆**&#x200B;摘要；請關閉摘要以返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-08.png)

## 在示範中試用

探索[Frescopa示範](https://play.llmo.now/org/demo-org)中的「新增相關常見問答」工作流程。
