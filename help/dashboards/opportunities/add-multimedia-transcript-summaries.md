---
title: 新增多媒體逐字稿摘要
description: 了解 LLM Optimizer 如何識別將關鍵資訊嵌入於影片中且缺乏機器可讀文字的頁面，以及如何透過邊緣最佳化審閱及部署 AI 生成的逐字稿摘要。
feature: Opportunities
autotag-review: '2026-05-15T17:28:28.569Z'
TQID: 'https://experienceleague.adobe.com/LiXMsMq6D08ciXR85aQBNDpmR5Csiv-5b9kv3lfTpDc'
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
source-wordcount: 775
ht-degree: 100%

---


# 新增多媒體逐字稿摘要

>[!NOTE]
>
> **搶先體驗** —「新增多媒體逐字稿摘要」功能目前處於搶先體驗階段。 隨著此功能逐漸成熟，其可用性、適用資格及部分工作流程可能會有所變更。 如果您對存取權有任何疑問，請聯絡您的 Adobe 帳戶團隊。

「新增多媒體逐字稿摘要」機會可識別重要資訊位於影片或其他媒體內，但是缺乏 AI 代理可讀取的逐字稿或簡短文字摘要的頁面。 此機會引進以媒體內容及周邊頁面內容為基礎的 **AI 生成逐字稿摘要**。 此機會將多媒體內容變成 AI 代理可理解的形式，協助復原原本可能被遺漏的重要品牌資訊。

針對每個受影響的 URL，您可以審閱建議的&#x200B;**「內容修補」**、「**實施**&#x200B;詳細資訊」(例如，目標 CSS 選擇器和作業) 和&#x200B;**「理由」**，然後透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)進行部署，讓代理式流量接收經擴充的 HTML，且無需變更內容管理系統 (CMS)。

## 其修正問題的方式

將透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)套用修正，且具備以下特點：

- 為 AI 代理提供預先轉譯的 HTML 快照。
- 在擷取的 HTML 中加入逐字稿摘要文字，以擴充頁面內容 (例如顯示於相關內嵌影片附近)。
- 在內容傳遞網路層運作 (無需變更 CMS)。
- 僅針對 AI 運作，不會影響真人訪客或搜尋引擎最佳化機器人。
- 只需幾分鐘即完成部署，且可透過 LLM Optimizer 介面&#x200B;**完全還原**。

## 運作方式

LLM Optimizer 會根據您的設定與頁面結構，標示嵌入式媒體缺乏機器可讀文字的高流量頁面。 受影響的 URL 會顯示在&#x200B;**「目前建議」**&#x200B;標籤的&#x200B;**「有建議項目的 URL」**&#x200B;表格中，您可以顯示資料列來檢視每個&#x200B;**「內容修補」**、其套用方式，以及建議使用的原因。

每個頁面皆有以下項目：

**多媒體摘要** – 根據影片內容產生的結構化摘要。
**預覽** – 套用前與套用後的頁面比較。

![目前建議中的有建議項目的 URL，已顯示的資料列以及內容修補、實施詳細資訊與理由](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-expand.png)

**「有建議項目的 URL」**&#x200B;表格會列出加入逐字稿或摘要文字後有助於代理式搜尋的頁面。 建議項目分為&#x200B;**「目前建議」**、**「已修正建議」**&#x200B;和&#x200B;**「已忽略建議」**。 針對每個 URL，您可以：

- **顯示資料列**&#x200B;以檢視&#x200B;**「內容修補」**&#x200B;的文字、「**實施**&#x200B;詳細資訊」(包括規劃的 DOM 作業和 CSS 選擇器) 以及變更的&#x200B;**「理由」**。
- **預覽**&#x200B;代理式流量變更前後的比較。
- **標記為已修正**，如果您是在 LLM Optimizer 之外處理機會。
- **忽略**&#x200B;不相關的建議。

若支援編輯功能 (鉛筆圖示控制項)，您可直接在資料列中編輯修補文字，然後使用該列的核取方塊選取要部署的項目。 頁尾會顯示已選取項目的數量，並提供&#x200B;**「標記為已修正」**、**「忽略建議」**&#x200B;和&#x200B;**「部署最佳化項目」**。

### 部署最佳化項目

當您準備好發佈至邊緣時，請按一下&#x200B;**「部署最佳化項目」**。 **「部署至邊緣」**&#x200B;對話框會列出您即將執行的 URL、選擇器和作業。 審閱清單，然後選擇&#x200B;**「部署」**&#x200B;或&#x200B;**「取消」**。

![用於多媒體逐字稿摘要內容修補的部署至邊緣對話框](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-dialog.png)

成功部署後，**「部署完成」**&#x200B;會確認有多少最佳化項目已上線。 關閉對話框並開啟&#x200B;**「已修正建議」**&#x200B;以驗證狀態。

![部署完成確認](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-confirm.png)

>[!NOTE]
>
> 部署最佳化項目之前，必須先完成邊緣最佳化的上線流程。 如果您尚未上線，按一下&#x200B;**「部署最佳化項目」**，系統便會將您導向上線流程。 如需了解邊緣最佳化的運作方式、支援的內容傳遞網路提供者和上線流程的完整詳細資訊，請參閱[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

### 已修正建議和檢視即時版本

在&#x200B;**「已修正建議」**&#x200B;中，已部署的 URL 會在狀態欄顯示&#x200B;**「已最佳化」**。 顯示資料列以審閱即時&#x200B;**「內容修補」**、「**實施**&#x200B;詳細資訊」和&#x200B;**「理由」**。 此外，您可以將&#x200B;**「詳細資訊」**&#x200B;用於分析，或使用&#x200B;**「檢視即時版本」** (若可用) 確認代理式流量會收到哪些內容。

![已修正建議和已最佳化狀態、已顯示的內容修補，以及復原](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-fixed.png)

若要大量復原，請使用核取方塊選取已最佳化的資料列，然後使用標頭中的&#x200B;**「復原」**。

![已修正建議，顯示復原前已選取的資料列](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-select-in-fixed.png)

## 復原

如果您改變主意，可以復原已部署的最佳化項目。 在&#x200B;**「已修正建議」**&#x200B;中，選取要還原的列，然後按一下&#x200B;**「復原」**。

**「復原」**&#x200B;對話框會列出將要復原的建議並提出警告，表示已部署的最佳化項目將從代理式流量的即時路徑中移除。 確認後，按一下&#x200B;**「復原」**&#x200B;或&#x200B;**「取消」**。

![復原對話框列出將還原的建議](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-dialog.png)

操作完成後將會顯示&#x200B;**「已成功復原」**&#x200B;摘要；關閉摘要即可返回儀表板。

![復原完成 — 已成功復原](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-confirm.png)

## 在示範中試用

探索[ Frescopa 示範](https://play.llmo.now/org/demo-org)中的「新增多媒體逐字稿摘要」工作流程。
