---
title: 最佳化機會
description: 了解如何使用機會儀表板，以自動偵測您的網站可以如何針對提升品牌能見度進行改善。
feature: Opportunities
autotag-review: '2026-05-15T17:53:48.623Z'
TQID: 'https://experienceleague.adobe.com/FAbQhzuyT-kIitIaoVQ47dam-TpN-deU5Vbo1nmK5CA'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: a0b5a505-2fd7-4c3d-b61c-b557fb6f0558
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 1227
ht-degree: 100%

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
| [新增適合 LLM 讀取的摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md) | 內容 (站內) | 識別在頁面或區段層級缺乏簡潔摘要與結構化重點，導致 AI 代理難以掃描及解讀品牌聲明的高流量頁面。 顯示受影響的 URL，以及建議新增摘要的位置。 | 審閱以現有內容為基礎並由 AI 生成的摘要及重點，然後透過邊緣最佳化部署至內容傳遞網路邊緣，讓 AI 代理取得更清晰且易於掃描的內容。 |
| [新增相關常見問題集](/help/dashboards/opportunities/add-relevant-faqs.md) | 內容 (站內) | 識別因缺少與提示集相符的結構化常見問題集內容，導致 AI 代理難以將使用者問題對應至相關頁面的高流量頁面。 顯示受影響的 URL，以及建議新增常見問題集的位置。 | 審閱以現有頁面內容為基礎、與意圖相符並由 AI 生成的常見問題集內容，然後透過邊緣最佳化部署至內容傳遞網路邊緣，讓 AI 代理取得更清晰的常見問題集背景資訊。 |
| [新增多媒體逐字稿摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md) | 內容 (站內) | 識別將關鍵資訊嵌入影片或其他媒體內，但因缺少機器可讀逐字稿或摘要，導致 AI 代理難以使用該內容的頁面。 顯示受影響的 URL 和建議的文字。 | 審閱以媒體內容與頁面為基礎並由 AI 生成的逐字稿摘要，然後透過邊緣最佳化部署至內容傳遞網路邊緣，讓 AI 代理取得機器可讀的文字 (例如顯示於相關影片附近)。 |
| [遭 robots.txt 封鎖的流量](/help/dashboards/opportunities/traffic-blocked-by-robots.md) | 技術性 GEO | 分析您的 robots.txt 檔案，找出有哪些規則選擇性封鎖 AI 代理，使其無法存取其他可公開存取之內容。 報告受影響的 URL 與遭封鎖的 AI 代理。 | 更新您的 robots.txt 檔案，以便在適當的時候允許支援的 AI 爬蟲存取。 |
| [代理式流量錯誤](/help/dashboards/opportunities/agentic-traffic-errors.md) | 技術性 GEO | 監視傳回 AI 代理的 404、403 和 5xx 錯誤回應的內容傳遞網路記錄。 報告受影響的 URL 和流失的點擊總數。 | 修正故障的連結、更新權限並解決伺服器端的問題，以便主要內容傳回 200 回應。 |
| [簡化複雜的內容](/help/dashboards/opportunities/simplify-complex-content.md) | 內容 (站內) | 識別因內容過於密集或複雜致使達不到可讀性門檻，導致 AI 代理難以解讀關鍵資訊的高流量頁面。 顯示受影響的 URL 以及建議使用簡化文字的位置。 | 審閱以現有頁面內容為基礎並由 AI 生成的改良文本，然後透過邊緣最佳化部署至內容傳遞網路邊緣，讓 AI 代理取得更清晰且更易於瀏覽的文字段落。 |
| [復原內容能見度](/help/dashboards/opportunities/recover-content-visibility.md) | 技術性 GEO | 標示對 AI 代理隱藏重要內容的頁面。 顯示受影響的 URL 和可復原的預期內容。 | 使用邊緣最佳化在內容傳遞網路層預先轉譯頁面，以便 AI 代理無需執行 JavaScript 即可獲得更多內容。 |
| [新增目錄](/help/dashboards/opportunities/add-table-of-contents.md) | 技術性 GEO | 偵測因為缺少清晰結構或導覽標題，導致 AI 代理難以剖析內容並將內容對應至使用者查詢的頁面。 顯示受影響的 URL 以及建議使用結構化目錄的位置。 | 審閱建議的結構化目錄，其包含可反映頁面主要區段的錨點連結標題，然後透過邊緣最佳化部署至內容傳遞網路邊緣，將目錄插入 HTML 中，以改善頁面結構，讓模型更容易擷取、對應及引用相關區段。 |
| [維基百科分析](/help/dashboards/opportunities/wikipedia-analysis.md) | 站外 | 根據參考資料、區段、內容長度、影像及資訊框內容完整性等面向，分析貴公司與業界競爭者的維基百科頁面。 找出您的頁面低於產業基準的具體落差。 | 審閱為改善您的維基百科存在感由 AI 生成的策略性建議，包括新增參考資料、補強資訊框內容、擴充區段及提升條目品質。 |
| [YouTube 情緒分析 (Beta)](/help/dashboards/opportunities/youtube-sentiment-analysis.md) | 站外、社交和社群 | 針對您的品牌存在感提示集所引用的 YouTube 影片，分析其中的品牌提及次數、情緒、聲量佔比及重複主題。 唯有系統偵測到 YouTube 影片被列為您的提示集之引用來源時才會顯示。 | 審閱可改善 YouTube 內容中品牌知覺的優先處理建議項目，包括建議採取的動作及負責實施的團隊。 |
| [Reddit 情緒分析 (Beta)](/help/dashboards/opportunities/reddit-sentiment-analysis.md) | 站外、社交和社群 | 針對您的品牌存在感提示集所引用的 Reddit 對話串，分析其中的品牌提及次數、情緒、聲量佔比及重複主題。 唯有系統偵測到 Reddit 對話串被列為您的提示集之引用來源時才會顯示。 | 審閱可改善 Reddit 內容中品牌知覺的優先處理建議項目，包括建議採取的動作及負責實施的團隊。 |
| [引用情緒分析 (Beta)](/help/dashboards/opportunities/cited-sentiment-analysis.md) | 站外、社交和社群 | 針對您的品牌存在感提示集中偵測到的熱門引用 URL，分析其中的品牌提及次數、情緒、聲量佔比及重複主題。 | 審閱優先處理的建議項目，針對 AI 系統在回答與您品牌相關的提示時最常引用的頁面，改善其中的品牌知覺。 |
| [擴充產品目錄 (Beta)](/help/dashboards/opportunities/enrich-product-catalog.md) | 內容 (站內)，Adobe Commerce | 識別名稱或說明過於籠統、技術資訊量極大或意義模糊，導致 LLM 難以解讀的 Commerce 目錄產品。 顯示已評估的 PDP、代理式流量背景資訊，以及 AI 生成的敘事強化內容。 | 審閱並編輯建議的產品名稱與說明，然後部署最佳化項目，將更新直接發佈至您的 Adobe Commerce 目錄 (可從「已修正建議」中復原)。 |
| [擴充產品詳細資訊頁面](/help/dashboards/opportunities/enrich-product-detail-pages.md) | 技術性生成式引擎最佳化，Adobe Commerce | 針對 Adobe Commerce 店面，比較完整目錄資料與 AI 代理在各產品詳細資訊頁面中可存取的內容；並顯示 AI 代理可見 HTML 中缺少變體、規格、屬性及相關目錄欄位的 PDP，並依代理式流量排定優先順序。 | 強調 AI 代理視圖中遺失的可復原目錄資訊，以及其對 LLM 驅動產品搜尋的重要性；並透過邊緣最佳化部署，在內容傳遞網路邊緣為代理式流量提供完整預先轉譯、適合 AI 使用的 HTML 快照，讓 AI 代理無需變更 CMS 或目錄，即可取得來自您目錄的豐富產品背景資訊。 |

## 自動最佳化 {#auto-optimization}

透過自動最佳化，可以一鍵部署所建議的最佳化，減少手動作業並縮短實現價值的時間。 可以在內容來源或是內容傳遞網路邊緣套用最佳化。 預先體驗版目前針對特定機會提供邊緣型自動最佳化。 如需詳細資訊，請參閱 [邊緣最佳化](/help/dashboards/optimize-at-edge/overview.md) 頁面。

<!--
### Recover Content Visibility Opportunity {#recover-contet}

As stated above, the content visibility opportunity, flags pages where key content is lost for AI agents due to client-side rendering. For each identified page, it shows you exactly which content is missing from the AI agent view, helping you pinpoint visibility gaps. It's also supported by an edge-based pre-rendering capability that can serve more HTML content to agentic traffic without requiring Content Management System (CMS) changes. This functionality is currently in Early Access and requires setup from the LLM Optimizer team. Please contact `llmo-at-edge@adobe.com` to activate the content visibility opportunity.
-->

### 其他工具

[LLM 能見度檢查程式](https://chromewebstore.google.com/detail/is-your-webpage-citable/jbjngahjjdgonbeinjlepfamjdmdcbcc)是 Chrome 擴充功能，讓您精準了解 LLM 可以存取您的網站多少內容，以及哪些內容仍是隱藏狀態。 這是一項免費使用的獨立診斷工具，不需要產品授權或設定。 只要點按一次，使用者就可以評估任何網站的電腦可讀性，用並排比較的方式查看 AI 代理與真人使用者看到的內容。 此外，也會預估使用 LLM Optimizer 可以復原多少內容。

<!--
| Detect Missing Hreflang | Content (Onsite)| Flags pages missing hreflang attributes. Provides affected URLs and expected coverage by language/region.| Implement hreflang tags to indicate correct localized versions. |
| Detect Missing Canonicals | Content (Onsite) | Scans for pages without canonical tags or with conflicting tags. Lists affected URLs and duplicates. | Add canonical tags pointing to the preferred version of each page. Ensure consistent usage across variants. |
-->
