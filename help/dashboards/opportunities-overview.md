---
title: 最佳化機會
description: 了解如何使用機會儀表板，以自動偵測您的網站可以如何針對提升品牌能見度進行改善。
feature: Opportunities
source-git-commit: b9e18081cd364b35a5375975cad949b7037bfaaf
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 31%

---


# 最佳化機會

最佳化機會是自動偵測而獲取的洞察，顯示您的網站和外部存在感可以改進哪些地方，以利在 AI 搜尋中提升品牌能見度。

這些最佳化包括修正頁面上的內容 (新增結構化內容、標準網址或摘要)、進行技術調整 (取消封鎖 AI 爬蟲或解決錯誤) 以及影響第三方權威網站上的內容。 把握這些最佳化機會有助於精準呈現您的品牌，並提高在生成式回答中被引用的機會。

![最佳化機會](/help/dashboards/assets/oport.png)

## 機會儀表板

儀表板上顯示的最佳化機會是根據競爭對手差距、趨勢上升的主題和效能資料來排定優先順序，並以清單方式呈現。 您可以使用搜尋欄位搜尋特定機會。 機會也會使用標籤分類，所以您可以直接點按標籤，即可顯示在該標籤之下的所有機會。

按一下「**詳細資料**」會開啟另一個視窗，並提供更多資訊和其他指引。

## 支援的機會

以下表格列出目前支援的機會：

| 機會 | 類型 | 發現的問題 | 修正建議 |
|---------|----------|----------|----------|
| [新增LLM易記摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md) | 內容 (站內) | 會識別在頁面或區段層級缺乏簡潔摘要和結構化關鍵點的高流量頁面，讓AI代理程式更難以掃描和解讀品牌宣告。 顯示受影響的URL以及建議摘要的位置。 | 檢閱AI產生的摘要和以現有內容為根據的關鍵點，然後透過Edge中的最佳化在CDN邊緣部署，讓代理程式獲得更清晰、可快速瀏覽的內容。 |
| [新增相關常見問題](/help/dashboards/opportunities/add-relevant-faqs.md) | 內容 (站內) | 會識別缺少與您的提示集對齊的結構化問答內容的高流量頁面，使AI代理程式更難以將使用者問題比對到您的頁面。 顯示受影響的URL以及建議常見問題的位置。 | 檢閱以現有頁面資料為基礎AI產生、與意圖一致的常見問題集內容，然後在CDN邊緣使用Edge中的最佳化進行部署，讓代理程式獲得更清楚的問答內容。 |
| [新增多媒體成績單摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md) | 內容 (站內) | 識別關鍵資訊嵌入到沒有機器可讀成績單或摘要的視訊或其他媒體中的頁面，使得AI代理程式難以使用該內容。 顯示受影響的URL和建議的文字。 | 檢閱以媒體和頁面為基礎的AI產生的成績單摘要，然後透過Edge中的最佳化在CDN邊緣部署，讓代理程式接收機器可讀的文字（例如，在相關視訊附近）。 |
| [由robots.txt封鎖的流量](/help/dashboards/opportunities/traffic-blocked-by-robots.md) | 技術性 GEO | 分析您的robots.txt檔案，找出可選擇性封鎖AI代理程式的其他公開存取內容的規則。 報告受影響的URL和封鎖的代理。 | 更新您的robots.txt檔案，在適當時允許存取支援的AI爬蟲。 |
| [代理流量錯誤](/help/dashboards/opportunities/agentic-traffic-errors.md) | 技術性 GEO | 監視傳回給AI代理程式的404、403和5xx錯誤回應的CDN記錄。 報表會影響URL和遺失的點選總數。 | 修正故障的連結、更新權限並解決伺服器端的問題，以便主要內容傳回 200 回應。 |
| [簡化複雜內容](/help/dashboards/opportunities/simplify-complex-content.md) | 內容 (站內) | 會識別高流量頁面，其中密集的或複雜的復本低於可讀性臨界值，讓AI代理程式更難解讀關鍵資訊。 顯示受影響的URL以及建議使用簡化文字的位置。 | 檢閱AI產生的改良文字以現有頁面內容為基礎，然後使用Edge中的最佳化在CDN邊緣部署，讓代理程式接收更清晰、更易於掃描的段落。 |
| [復原內容可見度](/help/dashboards/opportunities/recover-content-visibility.md) | 技術性 GEO | 標示對 AI 代理隱藏重要內容的頁面。 顯示受影響的 URL 和可復原的預期內容。 | 在CDN層使用在Edge最佳化來預先轉譯頁面，以便人工智慧代理程式無需執行JavaScript即可使用更多內容。 |
| [新增目錄](/help/dashboards/opportunities/add-table-of-contents.md) | 技術性 GEO | 偵測缺少清晰結構組織或導覽標題的頁面，導致AI代理程式難以剖析內容並將內容對應到使用者查詢。 顯示受影響的URL以及建議使用結構化目錄的位置。 | 檢閱建議的結構化目錄，其錨點連結標題可反映頁面的主要區段，然後在CDN邊緣部署並使用Edge中的最佳化，將目錄插入HTML，改善頁面結構，讓模型可更輕鬆擷取、對應和引用相關區段。 |
| [Wikipedia分析](/help/dashboards/opportunities/wikipedia-analysis.md) | 離站 | 跨參考、區段、內容長度、影像和資訊方塊完整性，針對業界競爭者分析貴公司的Wikipedia頁面。 找出您的頁面低於產業基準的特定差距。 | 檢閱AI產生的策略建議以改善您的維基百科存在，包括新增參考、豐富您的資訊箱、擴展區段及改善文章品質。 |
| [情緒分析(Beta)](/help/dashboards/opportunities/youtube-sentiment-analysis.md) | 異地、社交和社群 | 分析針對您的品牌存在感提示集所引用的YouTube影片，其中包含品牌提及、情緒、聲音份額和循環主題。 只有在偵測到YouTube影片是提示集的引文時才會顯示。 | 檢閱優先建議，以改善跨YouTube內容的品牌認知，包括建議的動作和負責實作這些動作的團隊。 |
| [Reddit情緒分析(Beta)](/help/dashboards/opportunities/reddit-sentiment-analysis.md) | 異地、社交和社群 | 分析針對品牌提及、情緒、聲音份額和循環主題的品牌存在感提示集引用的Reddit對話串。 只有在偵測到Reddit執行緒為提示集的引文時才會顯示。 | 審查優先順序的建議以改善Reddit內容的品牌認知度，包括建議的行動以及負責實施這些行動的團隊。 |
| [引用的情緒分析(Beta)](/help/dashboards/opportunities/cited-sentiment-analysis.md) | 異地、社交和社群 | 針對品牌提及、情緒、聲音份額和循環主題的品牌存在感提示集，分析偵測到的熱門引用URL。 | 檢閱優先建議，以改善人工智慧系統在回應有關您品牌的提示時引述最多的頁面的品牌知覺。 |
| [豐富產品目錄(Beta)](/help/dashboards/opportunities/enrich-product-catalog.md) | 內容（站上），Adobe Commerce | 識別其名稱或說明過於通用、技術密集或模糊以供LLM解讀的Commerce目錄產品。 顯示評估過的PDP、代理流量內容和AI產生的敘述強化。 | 檢閱並編輯建議的產品名稱和說明，然後部署最佳化以直接將更新發佈到您的Adobe Commerce目錄（從已修正的建議復原）。 |
| [豐富產品詳細資料頁面](/help/dashboards/opportunities/enrich-product-detail-pages.md) | 技術地理位置，Adobe Commerce | 對於Adobe Commerce店面，比較完整目錄資料與AI代理程式在每個產品詳細資料頁面上可以存取的內容；會顯示PDP，其中代理程式可見的HTML中缺少變體、規格、屬性和相關目錄欄位，而依代理程式流量來排定優先順序。 | 強調代理程式檢視中遺失的可復原目錄資訊，以及這對於以LLM驅動的產品探索來說很重要的原因；在Edge中使用最佳化進行部署，以便向CDN邊緣的代理程式流量提供完全預先呈現、適合AI的HTML快照，讓代理程式接收來自您目錄的豐富產品內容，而無需CMS或目錄變更。 |

## 自動最佳化 {#auto-optimization}

透過自動最佳化，可以一鍵部署所建議的最佳化，減少手動作業並縮短實現價值的時間。 可以在內容來源或是內容傳遞網路邊緣套用最佳化。 預先體驗版目前針對特定機會提供邊緣型自動最佳化。 如需詳細資訊，請參閱 [邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md) 頁面。

<!--
### Recover Content Visibility Opportunity {#recover-contet}

As stated above, the content visibility opportunity, flags pages where key content is lost for AI agents due to client-side rendering. For each identified page, it shows you exactly which content is missing from the AI agent view, helping you pinpoint visibility gaps. It's also supported by an edge-based pre-rendering capability that can serve more HTML content to agentic traffic without requiring Content Management System (CMS) changes. This functionality is currently in Early Access and requires setup from the LLM Optimizer team. Please contact `llmo-at-edge@adobe.com` to activate the content visibility opportunity.
-->

### 其他工具

[LLM 能見度檢查程式](https://chromewebstore.google.com/detail/is-your-webpage-citable/jbjngahjjdgonbeinjlepfamjdmdcbcc)是 Chrome 擴充功能，讓您精準了解 LLM 可以存取您的網站多少內容，以及哪些內容仍是隱藏狀態。 這是一項免費使用的獨立診斷工具，不需要產品授權或設定。 只要點按一次，使用者就可以評估任何網站的電腦可讀性，用並排比較的方式查看 AI 代理與人類使用者看到的內容。 此外，也會預估使用 LLM Optimizer 可以復原多少內容。

<!--
| Detect Missing Hreflang | Content (Onsite)| Flags pages missing hreflang attributes. Provides affected URLs and expected coverage by language/region.| Implement hreflang tags to indicate correct localized versions. |
| Detect Missing Canonicals | Content (Onsite) | Scans for pages without canonical tags or with conflicting tags. Lists affected URLs and duplicates. | Add canonical tags pointing to the preferred version of each page. Ensure consistent usage across variants. |
-->
