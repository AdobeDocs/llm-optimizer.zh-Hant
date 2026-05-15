---
title: 復原內容能見度
description: 瞭解LLM Optimizer如何識別對AI代理程式隱藏關鍵內容的頁面，以及如何使用邊緣型最佳化復原該可見性。
feature: Opportunities
autotag-review: '2026-05-15T17:56:37.098Z'
TQID: 'https://experienceleague.adobe.com/rHqJL4RrJr1ghsy4fhXe-JLDrWruNSZgVhXQeRN-iyA'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: a0b5a505-2fd7-4c3d-b61c-b557fb6f0558
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 928
ht-degree: 1%

---


# 復原內容能見度

AI代理程式只能引用其可存取的內容。 當頁面上的關鍵內容隱藏在使用者端轉譯和動態載入（例如產品說明、使用者評等、食譜和評論）後面時，AI代理程式就會完全遺漏這些內容，導致有價值的內容無法顯示給可能引述這些內容的系統。

復原內容可見度機會可識別您網站上存在此可見性差距的頁面。 對於每個受影響的頁面，它都會精確顯示AI代理程式檢視中遺漏哪些內容、強調間隙，並讓您套用修正專案，而不需要任何CMS變更或開發人員參與。

它一眼就能呈現三個關鍵量度：

- **URL** — 以內容可見度間隙識別的頁數。
- **預估內容增益** — 可以套用最佳化來復原的預估內容乘數。
- **平均內容可見度** — AI代理程式目前可在受影響頁面上看到的平均內容百分比。

![復原內容可見度儀表板](/help/dashboards/opportunities/assets/recover-content-visibility-overview.png)

如需此機會的影片概觀，您可以觀看[復原內容可見度](https://www.youtube.com/watch?v=BigPyJssFCw)。

在Edge[&#128279;](/help/dashboards/optimize-at-edge/overview.md)使用最佳化，即可最佳化此機會。 最佳化只提供給AI代理程式，對人類的訪客沒有影響（僅限機器人的傳遞）。 最佳化接著會在CDN層套用，不需進行CMS變更，且數分鐘內即可生效，不需開發人員參與，這是一項快速、低風險的部署。

## 運作方式

LLM Optimizer會比較AI代理程式可以存取的內容與頁面上實際顯示的內容，藉此分析您的頁面。 接收高代理流量但具有低內容可見度的頁面會出現在具有建議&#x200B;**表格的** URL中，並以代理流量為優先順序。

對於每個受影響的URL，LLM Optimizer都會提供：

- **AI分析** — 說明遺漏了哪些內容以及哪些內容對LLM可轉譯性很重要，並提供可復原的內容參考清單
- **內容可見度** — 該頁面上AI代理程式目前可看到的內容百分比
- **內容增益比率** — 套用最佳化時可復原內容的預估乘數
- **預覽** — 並排的HTML比較，顯示頁面現在與最佳化後的外觀，讓您在部署之前驗證變更

此修正是使用[在Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)套用的 — Adobe的邊緣型部署功能，可在CDN層向LLM使用者代理程式提供完全預先轉譯、適合AI的HTML快照，在不接觸您CMS的情況下復原先前隱藏的內容。

<!-- [URLs with suggestions](/help/dashboards/opportunities/assets/recover-content-visibility-urls.png)-->

## 具有建議的 URL

含有建議的&#x200B;**URL**&#x200B;表格列出所有受影響的頁面，並可依分類篩選。 對於每個URL，您可以：

- **展開列**&#x200B;以檢視AI分析，包括缺少什麼內容以及它重要的原因。
- **預覽**&#x200B;目前頁面與最佳化後版本的並排HTML比較。
- 問題解決後&#x200B;**標籤為已修正**。
- **忽略不相關的**&#x200B;建議。

建議分為三個檢視： **目前的建議**、**已修正的建議**&#x200B;和&#x200B;**忽略的建議**。 部署建議後，建議會移至狀態為&#x200B;**最佳化**&#x200B;的「固定建議」，並會執行&#x200B;**檢視即時**&#x200B;動作，以驗證代理流量的最佳化已上線。 您也可以隨時復原已修正的建議。

![已修正最佳化狀態的建議](/help/dashboards/opportunities/assets/recover-content-visibility-fixed.png)

## 部署最佳化

檢閱建議並選取要最佳化的URL後，按一下&#x200B;**部署最佳化**&#x200B;以在CDN邊緣發佈修正。 **部署至Edge**&#x200B;確認對話方塊會顯示選取的URL、其型別(Prerender)以及套用的建議。 部署後，確認畫面會確認哪些URL已成功最佳化。

>[!NOTE]
>
>部署最佳化需要在Edge上線流程中完成最佳化。 如果您尚未上線，按一下&#x200B;**部署最佳化**&#x200B;會將您導向到上線流程。 如需Edge最佳化如何運作、支援的CDN提供者和上線流程的完整詳細資訊，請參閱[Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面。

![部署至Edge對話方塊](/help/dashboards/opportunities/assets/recover-content-visibility-deploy.png)

## 在示範中試用

使用Frescopa示範環境檢視使用中的復原內容可見度機會。

[在Frescopa示範中檢視復原內容可見度](https://play.llmo.now/org/demo-org/opportunities/prerender/75729489-756a-4c2b-9905-155b1715da5c)

## 常見問題

**為什麼我的頁面內容對AI代理程式隱藏？**

大部分的現代網站都仰賴JavaScript在初始頁面請求後動態載入內容。 AI代理程式通常不會執行JavaScript，因此使用者端轉譯的內容（產品清單、使用者評論、部落格連結及類似元素）不會被AI代理程式看到，即便人類訪客可以完全看到該內容。

**此最佳化是否會影響我的人類訪客或SEO機器人？**

不是。 在Edge最佳化僅針對AI使用者代理。 人類訪客和SEO機器人接收原始頁面的方式與之前完全相同，且其體驗或效能不會有任何變更。

**我需要變更我的CMS或讓開發人員參與嗎？**

不是。 最佳化會套用在CDN邊緣，不需要編寫變更、程式碼部署或開發人員參與。 在Edge上線最佳化後，您可以直接從LLM Optimizer介面在幾分鐘內部署和復原變更。

**如果我部署後頁面內容變更，會發生什麼事？**

在復原內容可見度中，LLM Optimizer會使用低快取TTL設定，因此網站上的任何內容更新都會在數分鐘內觸發重新整理。 AI代理程式一律會收到您內容的最新版本。

**如何開始使用Edge最佳化？**

請參閱[在Edge最佳化](/help/dashboards/optimize-at-edge/overview.md)頁面，瞭解完整上線程式、CDN設定指南和先決條件。
