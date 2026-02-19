---
title: 最佳化機會
description: 了解如何使用機會儀表板，以自動偵測您的網站可以如何針對提升品牌能見度進行改善。
feature: Opportunities
source-git-commit: 3204d46106b4ae1645df19138cabd55bf153eb42
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 97%

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
| 建議結構化內容 (FAQ) | 內容 (站內) | 偵測缺乏相符的常見問答項目的高熱門度提示。 顯示相關的提示、類別和受影響的 URL。 | 新增常見問題結構描述區塊，內含符合常見查詢的簡潔回答。 |
| 偵測代理式流量被封鎖的情況 | 技術性 GEO | 分析內容傳遞網路的記錄，搜尋來自已知 AI 代理 (例如 GPTBot、PerplexityBot) 但被封鎖的要求。 報告受影響的 URL 和代理。 | 更新 robots.txt 或伺服器設定檔，以便在適當的時候允許支援的 AI 爬蟲存取。 |
| 偵測 404/403/5xx 的問題 | 技術性 GEO | 監視內容傳遞網路記錄是否出現錯誤回應。 報告錯誤頻率、受影響的 URL 以及預估遺失的點擊數。 | 修正故障的連結、更新權限並解決伺服器端的問題，以便主要內容傳回 200 回應。 |
| 簡化複雜的內容 | 內容 (站內) | 會識別超過可讀性臨界值的複雜長段落，這會降低AI理解度。 | 預先轉譯頁面，讓 AI 代理無需執行 JavaScript 即可取得更多內容。 |
| 復原內容能見度 (預先體驗版) | 技術性 GEO | 標示對 AI 代理隱藏重要內容的頁面。 顯示受影響的 URL 和可復原的預期內容。 | 預先轉譯頁面，讓 AI 代理無需執行 JavaScript 即可取得更多內容。 |

## 自動最佳化 {#auto-optimization}

透過自動最佳化，可以一鍵部署所建議的最佳化，減少手動作業並縮短實現價值的時間。 可以在內容來源或是內容傳遞網路邊緣套用最佳化。 預先體驗版目前針對特定機會提供邊緣型自動最佳化。 如需詳細資訊，請參閱 [邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md) 頁面。

<!--### Recover Content Visibility Opportunity {#recover-contet}

As stated above, the content visibility opportunity, flags pages where key content is lost for AI agents due to client-side rendering. For each identified page, it shows you exactly which content is missing from the AI agent view, helping you pinpoint visibility gaps. It's also supported by an edge-based pre-rendering capability that can serve more HTML content to agentic traffic without requiring Content Management System (CMS) changes. This functionality is currently in Early Access and requires setup from the LLM Optimizer team. Please contact `llmo-at-edge@adobe.com` to activate the content visibility opportunity.-->

### 其他工具

[LLM 能見度檢查程式](https://chromewebstore.google.com/detail/is-your-webpage-citable/jbjngahjjdgonbeinjlepfamjdmdcbcc)是 Chrome 擴充功能，讓您精準了解 LLM 可以存取您的網站多少內容，以及哪些內容仍是隱藏狀態。 這是一項免費使用的獨立診斷工具，不需要產品授權或設定。 只要點按一次，使用者就可以評估任何網站的電腦可讀性，用並排比較的方式查看 AI 代理與人類使用者看到的內容。 此外，也會預估使用 LLM Optimizer 可以復原多少內容。

<!--| Detect Missing Hreflang | Content (Onsite)| Flags pages missing hreflang attributes. Provides affected URLs and expected coverage by language/region.| Implement hreflang tags to indicate correct localized versions. |
| Detect Missing Canonicals | Content (Onsite) | Scans for pages without canonical tags or with conflicting tags. Lists affected URLs and duplicates. | Add canonical tags pointing to the preferred version of each page. Ensure consistent usage across variants. |-->
