---
title: 最佳化機會
description: 了解如何使用機會儀表板，以自動偵測您的網站可以如何針對提升品牌能見度進行改善。
feature: Opportunities
source-git-commit: 96bb7d73c8cdd2151df12030bbf28723857c78e1
workflow-type: tm+mt
source-wordcount: '780'
ht-degree: 59%

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
| 將長段落整理成摘要 | 內容 (站內) | 偵測超出建議長度臨界值的段落。 顯示受影響的 URL 和長度超標的文字片段。 | 建立摘要或將長段文字分割成較短、可掃描的區段。 |
| Recommend Structured Content | 內容 (站內) | 偵測缺乏相符的常見問答項目的高熱門度提示。 顯示相關的提示、類別和受影響的 URL。 | 新增常見問題結構描述區塊，內含符合常見查詢的簡潔回答。 |
| [Traffic Blocked by robots.txt](/help/dashboards/opportunities/traffic-blocked-by-robots.md) | 技術性 GEO | Analyzes your robots.txt file for rules that selectively block AI agents from content that is otherwise publicly accessible. Reports affected URLs and blocked agents. | Update your robots.txt file to allow access for supported AI crawlers where appropriate. |
| [Agentic Traffic Errors](/help/dashboards/opportunities/agentic-traffic-errors.md) | 技術性 GEO | Monitors CDN logs for 404, 403, and 5xx error responses returned to AI agents. Reports affected URLs and total hits lost. | 修正故障的連結、更新權限並解決伺服器端的問題，以便主要內容傳回 200 回應。 |
| 簡化複雜的內容 | 內容 (站內) | 找出超過可讀性臨界值，可能會降低 AI 理解度的長篇、複雜的段落。 | 預先轉譯頁面，讓 AI 代理無需執行 JavaScript 即可取得更多內容。 |
| [復原內容可見度](/help/dashboards/opportunities/recover-content-visibility.md) | 技術性 GEO | 標示對 AI 代理隱藏重要內容的頁面。 顯示受影響的 URL 和可復原的預期內容。 | Pre-render the pages at the CDN layer using Optimize at Edge so more content is available to AI agents without JavaScript execution. |
| [Wikipedia Analysis](/help/dashboards/opportunities/wikipedia-analysis.md) | Offsite | Analyzes your company&#39;s Wikipedia page against industry competitors across references, sections, content length, images, and infobox completeness. Identifies specific gaps where your page falls below industry benchmarks. | Review AI-generated strategic recommendations to improve your Wikipedia presence, including adding references, enriching your infobox, expanding sections, and improving article quality. |
| [YouTube Sentiment Analysis (Beta)](/help/dashboards/opportunities/youtube-sentiment-analysis.md) | Offsite, Social &amp; Community | Analyzes YouTube videos cited for your Brand Presence prompt set for brand mentions, sentiment, share of voice, and recurring topics. Only appears when YouTube videos are detected as citations for your prompt set. | Review prioritized recommendations to improve brand perception across YouTube content, including suggested actions and the teams responsible for implementing them. |
| [Reddit Sentiment Analysis (Beta)](/help/dashboards/opportunities/reddit-sentiment-analysis.md) | Offsite, Social &amp; Community | Analyzes Reddit threads cited for your Brand Presence prompt set for brand mentions, sentiment, share of voice, and recurring topics. Only appears when Reddit threads are detected as citations for your prompt set. | Review prioritized recommendations to improve brand perception across Reddit content, including suggested actions and the teams responsible for implementing them. |
| [引用的情緒分析(Beta)](/help/dashboards/opportunities/cited-sentiment-analysis.md) | 異地、社交和社群 | 針對品牌提及、情緒、聲音份額和循環主題的品牌存在感提示集，分析偵測到的熱門引用URL。 | 檢閱優先建議，以改善人工智慧系統在回應有關您品牌的提示時引述最多的頁面的品牌知覺。 |

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
