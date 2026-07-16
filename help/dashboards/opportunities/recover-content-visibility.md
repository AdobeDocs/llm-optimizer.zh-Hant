---
title: 復原內容能見度
description: 了解 LLM Optimizer 如何識別對 AI 代理隱藏關鍵內容的頁面，以及如何透過邊緣最佳化復原這些內容的能見度。
feature: Opportunities
autotag-review: '2026-07-15T18:06:12.342Z'
TQID: 'https://experienceleague.adobe.com/ILtulZHGNjX0Qes77aNZMe-Fs66BLryK3kHcORZ58A8'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
  - id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3a
subfeature_v2:
  - id: bbfc1b77-44c5-4fe8-b65f-ec160fe0d021
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 928
ht-degree: 100%

---


# 復原內容能見度

AI 代理只能引用其可存取的內容。 當頁面上的關鍵內容隱藏於用戶端轉譯與動態載入機制後方 (例如產品說明、使用者評分、配方與評論) 時，AI 代理會完全錯過這些內容，導致相關系統無法看見原本可能被引用的高價值內容。

「復原內容能見度」機會可識別您網站上存在這類能見度落差的頁面。 此機會針對每個受影響的頁面，清楚顯示 AI 代理視圖中缺少的內容、標示能見度落差，並讓您無需變更 CMS 或由開發人員介入即可套用修正。

此機會以一目了然的方式顯示三項關鍵量度：

- **URL**：發現有內容能見度落差的頁面數量。
- **預估的內容獲取量**：套用最佳化後可復原內容的預估倍數。
- **平均內容能見度**：AI 代理目前在受影響頁面中可見內容的平均百分比。

![復原內容能見度儀表板](/help/dashboards/opportunities/assets/recover-content-visibility-overview.png)

如需概述此機會的影片，您可以觀看[復原內容能見度](https://www.youtube.com/watch?v=BigPyJssFCw)。

此機會可透過](/help/dashboards/optimize-at-edge/overview.md)邊緣最佳化[進行最佳化。 最佳化內容只會提供給 AI 代理，且不會影響真人訪客 (僅對機器人傳遞)。 最佳化會直接在內容傳遞網路層套用，無需變更 CMS，且可在數分鐘內生效，也不需開發人員參與，是快速且低風險的部署方式。

## 運作方式

LLM Optimizer 會比較 AI 代理可存取的內容與頁面上實際存在的內容，藉此分析您的頁面。 接收大量代理式流量但內容能見度偏低的頁面，會顯示於&#x200B;**「有建議項目的 URL」**&#x200B;表格中，並以代理式流量多寡進行排序。

針對每個受影響的 URL，LLM Optimizer 都會提供：

- **AI 分析**：說明缺少哪些內容、其對 LLM 可引用性的重要性，以及可復原的內容參考清單。
- **內容能見度**：該頁面上目前 AI 代理可以看見的內容百分比
- **內容獲取比率**：套用最佳化後可復原內容的預估倍數
- **預覽**：並排顯示的 HTML 比較，可呈現頁面目前與最佳化後的樣貌，讓您在部署前驗證變更內容

此修正會透過[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)套用，這是 Adobe 的邊緣部署功能，可在內容傳遞網路層向 LLM 使用者代理提供完整預先轉譯、適合 AI 使用的 HTML 快照，無需變更 CMS，即可復原先前隱藏的內容。

<!-- [URLs with suggestions](/help/dashboards/opportunities/assets/recover-content-visibility-urls.png)-->

## 具有建議的 URL

**「有建議項目的 URL」**&#x200B;表格會列出所有受影響的頁面，並可依分類進行篩選。 針對每個 URL，您可以：

- **顯示資料列**&#x200B;以檢視 AI 分析，包括缺少哪些內容，以及這些內容為何重要。
- **預覽**&#x200B;目前頁面與最佳化後版本的並排 HTML 比較。
- 問題解決後&#x200B;**標記為已修正**。
- **忽略**&#x200B;不相關的建議。

建議項目分為三種視圖：**「目前建議」**、**「已修正建議」**&#x200B;以及&#x200B;**「已忽略建議」**。 建議項目經部署後便會移至「已修正建議」並顯示&#x200B;**「已最佳化」**&#x200B;狀態，並有&#x200B;**「檢視即時版本」**&#x200B;動作以驗證最佳化是否已對代理式流量生效。 您也可以隨時復原已修正的建議。

![已修正建議及已最佳化狀態](/help/dashboards/opportunities/assets/recover-content-visibility-fixed.png)

## 部署最佳化項目

審閱建議並選取要最佳化的 URL 後，按一下&#x200B;**「部署最佳化項目」**，即可將修正發佈至內容傳遞網路邊緣。 **「部署至邊緣」**&#x200B;確認對話框會顯示選取的 URL、其類型 (預轉譯) 以及套用的建議。 部署後，確認畫面會顯示哪些 URL 已成功完成最佳化。

>[!NOTE]
>
>部署最佳化項目之前，必須先完成邊緣最佳化的上線流程。 如果您尚未上線，按一下&#x200B;**「部署最佳化項目」**，系統便會將您導向上線流程。 如需了解邊緣最佳化的運作方式、支援的內容傳遞網路提供者和上線流程的完整詳細資訊，請參閱[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

![部署至邊緣對話框](/help/dashboards/opportunities/assets/recover-content-visibility-deploy.png)

## 在示範中試用

使用 Frescopa 示範環境查看「復原內容能見度」機會的實際運作情形。

[在 Frescopa 示範中檢視復原內容能見度](https://play.llmo.now/org/demo-org/opportunities/prerender/75729489-756a-4c2b-9905-155b1715da5c)

## 常見問題

**為什麼 AI 代理無法看見我的頁面內容？**

大多數現代網站都仰賴 JavaScript，在初始頁面請求後動態載入內容。 AI 代理通常不會執行 JavaScript，因此透過用戶端轉譯的內容 (例如產品清單、使用者評論、部落格文章連結及類似元素)，即使真人訪客可以完全看到這類內容，AI 代理也無法看見。

**此最佳化是否會影響真人訪客或搜尋引擎最佳化機器人？**

不是。 邊緣最佳化僅針對 AI 使用者代理。 真人訪客與搜尋引擎最佳化機器人仍會與以往一樣接收原始頁面，其使用體驗與效能不受影響。

**我需要變更我的 CMS 或讓開發人員參與嗎？**

不用。 最佳化項目會內容傳遞網路邊緣直接套用，無需進行編寫變更，亦無需部署程式碼或由開發人員參與。 完成邊緣最佳化上線後，您可以直接透過 LLM Optimizer 介面，在數分鐘內部署及復原變更。

**如果部署後頁面內容有所變更，會發生什麼事？**

對於「復原內容能見度」，LLM Optimizer 會使用低快取 TTL 設定，以便網站上任何內容的更新都會在數分鐘內觸發重新整理。 AI 代理總是會取得內容的最新版本。

**如何開始使用邊緣最佳化？**

請參閱[邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面，了解完整的上線程式、內容傳遞網路設定指南和先決條件。
