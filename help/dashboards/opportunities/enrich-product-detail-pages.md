---
title: 產品詳細資料頁面擴充
description: 瞭解LLM Optimizer如何識別對AI代理程式隱藏目錄資料的產品頁面，以及如何使用Adobe Commerce支援的邊緣型最佳化和產品目錄深入解析來復原可見性。
feature: Opportunities
autotag-review: '2026-05-15T17:46:41.487Z'
TQID: 'https://experienceleague.adobe.com/l4hTGNNg1NW40ceI00P41KZBSGcqmr-t1RWM-NXtRV4'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 1210
ht-degree: 0%

---


# 豐富產品詳細資料頁面

AI代理程式只能建議他們完全可以瞭解的產品。 在大多數商業店面，產品頁面是專為人類購物者設計的。 因此，這些產品依賴JavaScript轉譯的標籤、可展開的面板、購物精靈、互動式模組，以及表面產品變體、規格和功能的連結。 AI代理程式不會剖析產品詳細資料頁面的深度，這表示支援AI驅動探索的LLM爬蟲永遠不會看到這種豐富的產品資料，即便人類訪客可以完全看到這些資料。

豐富產品詳細資料頁面機會會識別您的Adobe Commerce目錄中存在此可見性差距的產品頁面。 它由Adobe Commerce目錄提供技術支援，會比較AI代理程式在店面可存取的內容與目錄中可用的完整產品資料，並顯示AI代理程式檢視中缺少的所有屬性、變體以及產品特性深度。

它讓您一眼就能瞭解下列關鍵量度：

- **產品頁面** — 所有以目錄資料可見性差距識別的產品詳細資料頁面的清單。
- **代理流量** — 網站上由代表使用者執行以探索、擷取或參與內容之自主AI代理程式（例如LLM支援的助理或機器人）所起始和驅動的造訪與互動總數。

![豐富產品詳細資料頁面](/help/dashboards/opportunities/assets/enrich-product-detail-pages-overview.png)

在Edge[&#128279;](https://experienceleague.adobe.com/en/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)使用最佳化，即可最佳化此機會。 最佳化作業會專門傳送給AI代理程式，不影響人類訪客（僅限機器人傳送），套用在CDN層而不需要CMS或目錄變更，且幾分鐘內即可生效，沒有開發人員參與，因此對於大型產品目錄而言是快速、低風險的部署路徑。

## 運作方式

Adobe Commerce目錄代理程式會讀取您的完整產品目錄資料，包括：變體、更深入的產品關係、屬性、Facet、類別中繼資料和所有產品特性。 然後，它會將這些資料與對應店面PDP上AI代理程式實際可存取的內容進行比較。 對AI爬蟲隱藏目錄資料的頁面會顯示在包含建議&#x200B;**表格的** URL中，並以代理流量為優先順序。

對於每個受影響的產品頁面，LLM Optimizer都提供：

- **AI Analysis預覽** — AI代理程式檢視中遺漏的目錄資訊完整清單，以及這些資訊對LLM驅動的產品探索重要的原因，包括可復原資料點的清單，例如產品變體、大小選項、材質規格和相容性詳細資訊等。

此修正是使用[在Edge最佳化](https://experienceleague.adobe.com/en/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)套用的 — Adobe的邊緣型部署功能，可在CDN層向LLM使用者代理程式提供完全預先呈現、適合AI使用的HTML快照。 如此一來，您無需接觸Commerce目錄或人類看得見的店面UI，即可復原所有先前隱藏的目錄資料（包括產品變體、技術規格和功能詳細資訊）。

![個含建議資料表的URL](/help/dashboards/opportunities/assets/enrich-product-detail-pages-suggestions.png)

## 具有建議的 URL

含有建議&#x200B;**的** URL表格列出所有已識別且受益於最佳化的產品頁面。 對於每個產品URL，您可以：

- **預覽**&#x200B;以檢視AI分析，包括缺少哪些目錄資訊以及它們對AI驅動的可發現性重要的原因
- 部署並驗證最佳化後，**標示為固定**
- **忽略與您銷售策略無關的**&#x200B;建議

建議分為三個檢視： **目前的建議**、**已修正的建議**&#x200B;和&#x200B;**忽略的建議**。 部署建議後，會移至狀態為&#x200B;**最佳化**&#x200B;且有&#x200B;**檢視即時**&#x200B;動作的「已修正建議」，以驗證代理流量的擴充功能是否處於即時狀態。 您可以隨時復原已修正的建議。

## 部署最佳化

檢閱建議並選取要最佳化的產品頁面後，按一下&#x200B;**部署最佳化**&#x200B;以在CDN邊緣發佈擴充。 **部署至Edge**&#x200B;確認對話方塊會顯示選取的產品URL、最佳化型別和正在套用的擴充。 部署後，確認畫面會確認哪些產品頁面已成功最佳化。

最佳化是透過CDN邊緣層專門傳遞給AI使用者代理。 人類訪客會繼續看見您現有的店面體驗，就像之前一樣，不會變更PDP設計、頁面效能或品牌體驗。

>[!NOTE]
>
>部署最佳化需要(1)將LLM Optimizer連線至Adobe Commerce，以及(2)完成Edge上線最佳化程式。

如果您的Commerce執行個體尚未連線至LLM Optimizer，系統會在套用擴充功能之前，將您導向連線設定。

如果您尚未上線，按一下&#x200B;**部署最佳化**&#x200B;會將您導向到上線流程。 如需Edge最佳化如何運作、支援的CDN提供者和上線流程的完整詳細資訊，請參閱[Edge最佳化](https://experienceleague.adobe.com/en/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)頁面。

![部署至Edge對話方塊](/help/dashboards/opportunities/assets/enrich-product-detail-pages-deploy.png)

## 在示範中試用

使用Frescopa示範環境檢視豐富產品詳細資料頁面使用中的機會。

[在Frescopa示範中檢視豐富產品詳細資料頁面](https://play.llmo.now/org/demo-org/opportunities/commerce-product-page-enrichment/4e8b0428-0893-4864-a00e-fc1d77fb3372?siteId=9ae8877a-bbf3-407d-9adb-d6a72ce3c5e3)

## 常見問題

**為什麼我的產品目錄資料對AI代理程式隱藏？**

Commerce店面是專為人類購物者所打造。 產品特性、變體、尺寸選項、材料詳細資訊和其他技術規格，經常會透過JavaScript驅動的互動出現，例如標籤、可摺疊面板、快顯模式、連結和購物精靈。 AI代理程式不會剖析產品詳細資料頁面的深度，因此LLM爬蟲不會看見所有這些資料，即使它們完全出現在您的產品目錄中亦然。 結果是，AI代理程式會根據可用實際產品資訊的一部分提供產品建議。

**此最佳化會復原哪些型別的產品資料？**

目錄代理程式會復原您的Adobe Commerce目錄中目前無法在AI代理程式的店面存取的所有產品資訊。 這包括產品字元、關係、變體（大小、顏色、組態）、技術規格和屬性、相容性詳細資料、類別中繼資料和Facet值。

**此最佳化是否會影響我的人類訪客、SEO機器人或店面效能？**

不是。 在Edge最佳化僅針對AI使用者代理。 人類訪客和SEO機器人收到的原始產品頁面與之前完全一樣，不會變更其體驗、頁面載入效能或店面設計。

**我需要變更我的Commerce目錄、CMS或讓開發人員參與嗎？**

不是。 最佳化會套用在CDN邊緣層，不需變更Adobe Commerce目錄、部署程式碼或開發人員參與。 在Edge上線最佳化後，您可以直接從LLM Optimizer介面在幾分鐘內部署和復原增強功能。

**如果我的產品資料在部署後變更，會發生什麼事？**

在豐富產品詳細資料頁面機會中，LLM Optimizer會使用低快取TTL設定，這樣Commerce目錄中的任何產品更新都會在數分鐘內觸發重新整理。 AI代理程式一律會收到最新的可用產品資料。
