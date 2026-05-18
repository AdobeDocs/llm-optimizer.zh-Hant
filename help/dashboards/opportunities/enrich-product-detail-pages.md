---
title: 產品詳細資訊頁面擴充
description: 了解 LLM Optimizer 如何識別對 AI 代理隱藏產品目錄資料的產品頁面，以及如何透過 Adobe Commerce 支援的邊緣型最佳化與產品目錄洞察來復原其能見度。
feature: Opportunities
autotag-review: '2026-05-15T17:46:41.487Z'
TQID: 'https://experienceleague.adobe.com/l4hTGNNg1NW40ceI00P41KZBSGcqmr-t1RWM-NXtRV4'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 1210
ht-degree: 100%

---


# 擴充產品詳細資訊頁面

AI 代理只能推薦其能完全理解的產品。 在大多數電商店面平台中，產品頁面都是為真人購物者設計的。 因此，這些產品依賴 JavaScript 轉譯的標籤、可顯示的面板、購物精靈、互動式強制回應視窗，以及用於顯示產品變體、規格與功能的連結來呈現資訊。 AI 代理不會深入剖析產品詳細資訊頁面的內容，因此支援 AI 驅動搜尋的 LLM 爬蟲完全無法看見這些豐富的產品資料，即使真人訪客可以完整看到這些資訊。

「擴充產品詳細資訊頁面」機會可識別 Adobe Commerce 目錄中存在這類能見度落差的產品頁面。 在 Adobe Commerce 目錄的支援下，此機會將比較 AI 代理在店面中可存取的內容與目錄中的完整產品資料，並指出 AI 代理視圖中缺少的所有屬性、產品變體及產品特性的詳細細節。

此機會以一目了然的方式顯示下列關鍵量度：

- **產品頁面**：經確認存在目錄資料能見度落差的所有產品詳細資訊頁面之清單。
- **代理式流量**：網站上由自主 AI 代理 (例如由 LLM 驅動的助理或機器人) 代表使用者發起並驅動，用於搜尋內容、擷取內容或與內容互動的總造訪次數與互動量。

![擴充產品詳細資訊頁面儀表板](/help/dashboards/opportunities/assets/enrich-product-detail-pages-overview.png)

此機會可透過](https://experienceleague.adobe.com/zh-hant/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)邊緣最佳化[進行最佳化。 最佳化項目只會提供給 AI 代理，不會影響真人訪客 (僅對機器人傳遞)，並在內容傳遞網路層進行套用且無需變更 CMS 或產品目錄，無需開發人員參與即可在數分鐘內生效，因此成為大型產品目錄快速且低風險的部署方式。

## 運作方式

Adobe Commerce 目錄代理會讀取您的完整產品目錄資料，包括：產品變體、更完整的產品關聯、屬性、篩選側欄、類別中繼資料，以及所有產品特性。 然後，將這些資料與 AI 代理在對應店面 PDP 中實際可存取的內容進行比較。 對 AI 爬蟲隱藏目錄資料的頁面會顯示於&#x200B;**「有建議項目的 URL」**&#x200B;表格中，並依代理式流量多寡排定優先順序。

針對每個受影響的產品頁面，LLM Optimizer 會提供：

- **AI 分析預覽**：顯示 AI 代理視圖中缺少的完整目錄資訊清單，以及這些資訊對 LLM 驅動的產品搜尋的重要性，內容包含可復原的資料點，例如產品變體、尺寸選項、材質規格與相容性詳細資訊等。

將透過[邊緣最佳化](https://experienceleague.adobe.com/zh-hant/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)套用修正，這是 Adobe 的邊緣型部署功能，可在內容傳遞網路層向 LLM 使用者代理提供完整預先轉譯、適合 AI 使用的 HTML 快照。 如此一來，您無需變更 Commerce 目錄或真人訪客可見的店面 UI，即可復原所有先前隱藏的目錄資料 (包括產品變體、技術規格與功能詳細資訊)。

![有建議項目的 URL 表格](/help/dashboards/opportunities/assets/enrich-product-detail-pages-suggestions.png)

## 有建議項目的 URL

**「有建議項目的 URL」**&#x200B;表格會列出所有已識別、可透過最佳化獲益的產品頁面。 針對每個產品 URL，您可以：

- **預覽**&#x200B;以檢視 AI 分析，包括缺少哪些目錄資訊，以及這些資訊對 AI 驅動可搜尋性的重要性。
- 部署並驗證最佳化項目後，**標記為已修正**
- **忽略**&#x200B;與您銷售策略無關的建議

建議項目分為三種視圖：**「目前建議」**、**「已修正建議」**&#x200B;以及&#x200B;**「已忽略建議」**。 建議項目經部署後便會移至「已修正建議」並顯示&#x200B;**「已最佳化」**&#x200B;狀態，並有&#x200B;**「檢視即時版本」**&#x200B;動作以驗證擴充是否已對代理式流量生效。 您可以隨時復原已修正建議。

## 部署最佳化項目

審閱建議並選取要最佳化的產品頁面後，按一下&#x200B;**「部署最佳化項目」**&#x200B;在內容傳遞網路邊緣發佈擴充內容。 **「部署至邊緣」**&#x200B;確認對話框會顯示已選取的產品 URL、最佳化類型和所套用的擴充內容。 部署後，確認畫面會顯示哪些產品頁面已成功完成最佳化。

最佳化項目會透過內容傳遞網路邊緣層，僅提供給 AI 使用者代理。 真人訪客仍然與以往一樣看到現有的店面體驗，PDP 設計、頁面效能與品牌體驗皆不受影響。

>[!NOTE]
>
>部署最佳化項目需要 (1) 將 LLM Optimizer 連接至 Adobe Commerce，以及 (2) 完成邊緣最佳化上線流程。

如果您的 Commerce 執行個體尚未連接至 LLM Optimizer，系統會在套用擴充項目之前將您導向連線設定。

如果您尚未上線，按一下&#x200B;**「部署最佳化項目」**，系統便會將您導向上線流程。 如需了解邊緣最佳化的運作方式、支援的內容傳遞網路提供者和上線流程的完整詳細資訊，請參閱[邊緣最佳化](https://experienceleague.adobe.com/zh-hant/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)頁面。

![部署至邊緣對話框](/help/dashboards/opportunities/assets/enrich-product-detail-pages-deploy.png)

## 在示範中試用

使用 Frescopa 示範環境查看「擴充產品詳細資訊頁面」機會的實際運作情形。

[在 Frescopa 示範中檢視擴充產品詳細資訊頁面](https://play.llmo.now/org/demo-org/opportunities/commerce-product-page-enrichment/4e8b0428-0893-4864-a00e-fc1d77fb3372?siteId=9ae8877a-bbf3-407d-9adb-d6a72ce3c5e3)

## 常見問題

**為什麼 AI 代理無法看見我的產品目錄資料？**

電商店面平台是專為真人購物者打造。 產品特性、產品變體、尺寸選項、材質詳細資訊及其他技術規格，通常會透過 JavaScript 驅動的互動功能呈現，例如索引標籤、可收合面板、快顯強制回應視窗、連結與購物精靈。 AI 代理不會深入檢視產品詳細資訊頁面的內容，因此即使這些資料完整存在於您的產品目錄，LLM 爬蟲也無法看到。 因此，AI 代理只能根據部分可取得的實際產品資訊來提供產品推薦。

**此最佳化項目可復原哪些類型的產品資料？**

目錄代理會恢復 Adobe Commerce 目錄中目前 AI 代理無法從店面平台存取的所有產品資訊。 這包括產品特性、產品關聯、產品變體 (尺寸、顏色、配置)、技術規格與屬性、相容性詳細資訊、類別中繼資料，以及側欄篩選值。

**這個最佳化項目是否會影響真人訪客、搜尋引擎最佳化爬蟲或店面表現？**

不是。 邊緣最佳化僅針對 AI 使用者代理。 真人訪客與搜尋引擎最佳化爬蟲接收到的原始產品頁面與先前完全相同，其使用體驗、頁面載入效能與店面設計皆不受影響。

**我需要變更 Commerce 目錄、CMS 或讓開發人員參與嗎？**

否。 最佳化項目會在內容傳遞網路邊緣層進行套用，無需變更 Adobe Commerce 目錄，亦無需部署程式碼或讓開發人員參與。 完成邊緣最佳化上線後，您即可直接透過 LLM Optimizer 介面，在數分鐘內部署及復原擴充內容。

**如果我的產品資料在部署後有所變更，會發生什麼事？**

在「擴充產品詳細資訊頁面」機會中，LLM Optimizer 會使用低快取 TTL 設定，因此 Commerce 目錄中的任何產品更新都會在數分鐘內觸發重新整理。 AI 代理總是會取得產品資料的最新版本。
