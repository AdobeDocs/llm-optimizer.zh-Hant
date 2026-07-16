---
title: 邊緣最佳化
description: 了解如何在不需要更動原始內容的情況下，在內容傳遞網路邊緣完成 LLM Optimizer 最佳化。
feature: Opportunities
autotag-review: '2026-07-15T18:10:00.249Z'
TQID: 'https://experienceleague.adobe.com/nRq5punuSnNb4XXIJzkO1NGF66tsyN1rdt-O9dd8tmU'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
  - id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3a
subfeature_v2:
  - id: bbfc1b77-44c5-4fe8-b65f-ec160fe0d021
  - id: a6256a78-8814-462c-9627-86699b39cee1
  - id: e0ec491f-fe51-42b6-801c-1c0dfcc0e64f
  - id: fe92ae96-fc87-4fea-96a0-adc06310d4f4
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
  - id: e9001ce2-5245-4a8e-8601-dd958009072f
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 3147
ht-degree: 99%

---


# 邊緣最佳化

本頁面將詳細介紹如何在不變更原始內容的情況下，在內容傳遞網路邊緣完成最佳化。 本頁面會說明上線流程、可用的最佳化機會，以及如何在邊緣進行自動最佳化。

## 什麼是邊緣最佳化？

邊緣最佳化是 LLM Optimizer 的一項邊緣型部署功能，可以針對 LLM 使用者代理提供適合 AI 的變更。 在目前的情境下，「邊緣」(Edge) 是指在內容傳遞網路層套用最佳化。 因為這是在內容傳遞網路層提供最佳化，不需要變更內容管理系統 (CMS) 的原始內容，因此原始的 CMS 維持不變。 藉由這樣的區隔，您不需要變動現有的發佈工作流程，亦可改善 LLM 能見度。 此一最佳化僅針對代理式流量，而不影響真人使用者或 SEO 機器人。 當 LLM Optimizer 偵測到最佳化頁面的機會時，使用者可以直接在內容傳遞網路邊緣部署修正。

相對於傳統修正方式需要複雜的工程技術，邊緣最佳化是速度更快、更精簡的替代方案。 如前所述，您只要完成一次性設定，便不需要變更平台或經歷漫長的開發週期亦可套用變更。 不需要開發人員參與，您可以在數分鐘內發佈改善功能。 透過這個方法，您不需要更動任何程式碼，便能讓您的網站針對 AI 代理進行最佳化。

邊緣最佳化是專為行銷、SEO、內容和數位策略團隊設計的功能。 業務使用者可藉助這項功能，完成在 LLM Optimizer 的完整歷程：發現機會、了解建議並輕鬆部署修正。 透過邊緣最佳化，使用者可以預覽變更、在內容傳遞網路邊緣快速部署變更，以及驗證最佳化是否已上線。 您可以在 LLM Optimizer 生態系統中追蹤效能。

### 主要優點

* **僅對 AI 交付：**&#x200B;僅將最佳化的 HTML 提供給 AI 代理，不會對真人訪客或 SEO 機器人造成影響。
* **更短的週期：**&#x200B;在幾分鐘內發佈變更，而不是花數週時間。 不需要變更平台或經歷漫長的工程設計週期。
* **可還原：**&#x200B;支援一鍵復原功能，可在數分鐘內還原頁面。
* **不會影響效能：**&#x200B;邊緣型最佳化和快取，讓網站延遲不受影響。
* **適用任何內容傳遞網路與 CMS：**&#x200B;可與任何內容傳遞網路設定和前端設定搭配使用，無論使用哪種內容管理系統。

### 邊緣最佳化支援哪些機會？

邊緣最佳化支援可以改善代理式網頁體驗的機會。 關於各個機會的詳細資訊，請參閱[機會儀表板](/help/dashboards/opportunities-overview.md)頁面和目前頁面的機會區段。

## 上線

<!--You should reach out to either your Adobe account team or the FDE team to start the onboarding process. Your IT or CDN team is also required to complete the pre-requisites and setup process. Additionally, you can also contact `llmo-at-edge@adobe.com` for further onboarding assistance.-->

在您的 LLM Optimizer 帳戶中開始上線程序：

1. 在「**客戶設定**」儀表板上，選取「**內容傳遞網路設定**」標籤。
1. 按一下「**上線內容傳遞網路**」。
   ![「內容傳遞網路設定」分頁](/help/overview/assets/cc-cdn.png)
1. 對於由 AEM Cloud Service 管理的 Fastly 客戶，路由採自助方式設定並可直接在 LLM Optimizer UI 中完成。 對於使用其他內容傳遞網路提供者的客戶，您的 IT/內容傳遞網路團隊必須完成必要的設定及滿足先決條件。 如需其他指引，您也可以參閱下方提供的內容傳遞網路指南範例。

>[!NOTE]
>請參閱以下涵蓋完整上線流程的逐步操作指南。 如有指南中未能解決的問題，您可以聯絡 `llmo-at-edge@adobe.com`。

您的 IT/內容傳遞網路團隊需滿足的要求：

* 在網站中的 robots.txt 檔案或機器人流量管理規則的允許清單中，加入 `*AdobeEdgeOptimize/1.0*` 使用者代理。
* 請確認在網域或內容傳遞網路層級並未封鎖頁面。
* 在內容傳遞網路中新增邊緣最佳化路由規則。
* 如果您的內容傳遞網路有 WAF 或機器人管理員規則，請將 `*AdobeEdgeOptimize/1.0*` 使用者代理加入允許清單。 如果需要其他驗證，請設定 `x-edgeoptimize-fetcher-key` 標頭。 以下每個 BYOCDN 指南都包含相關步驟。
* 在 LLM Optimizer 介面中確認邊緣最佳化路由。

下圖顯示使用邊緣最佳化進行 BYOCDN 設定時的請求處理流程：

![BYOCDN 請求處理流程](/help/assets/optimize-at-edge/byocdn-request-flow.png)

>[!IMPORTANT]
>路由必須在外部內容傳遞網路 (最靠近用戶端的內容傳遞網路) 完成設定。 如果您有多個內容傳遞網路，則只能在外部內容傳遞網路進行路由。

若要引導設定流程，請在下方選取您的內容傳遞網路提供者，並按照相應的設定指南操作。 請記住，您應該根據實際的上線設定調整這些範例。 我們建議先在較低階的環境套用變更。

### 內容傳遞網路設定指南

| 內容傳遞網路提供者 | 類型 | 指南 |
|---|---|---|
| AEM Cloud Service 管理的內容傳遞網路 (Fastly) | Adobe 管理 | [檢視設定指南](/help/dashboards/optimize-at-edge/aemcs-managed-cdn.md) |
| Fastly (BYOCDN) | 自備內容傳遞網路 | [檢視設定指南](/help/dashboards/optimize-at-edge/fastly-byocdn.md) |
| Akamai (BYOCDN) | 自備內容傳遞網路 | [檢視設定指南](/help/dashboards/optimize-at-edge/akamai-byocdn.md) |
| Cloudflare (BYOCDN) | 自備內容傳遞網路 | [檢視設定指南](/help/dashboards/optimize-at-edge/cloudflare-byocdn.md) |
| CloudFront (BYOCDN) | 自備內容傳遞網路 | [檢視設定指南](/help/dashboards/optimize-at-edge/cloudfront-byocdn.md) |
| Azure正門(BYOCDN) | 自備內容傳遞網路 | [檢視設定指南](/help/dashboards/optimize-at-edge/azure-front-door-byocdn.md) |

>[!NOTE]
>
>如果上述清單內並未列出您的內容傳遞網路提供者，或您在 LLM Optimizer 使用者介面中找不到您的網域或電子郵件，請聯絡 `llmo-at-edge@adobe.com` 取得上線協助。 完成設定後，您可以在 LLM Optimizer 中部署邊緣最佳化機會之建議。

上述每項內容傳遞網路設定指南在結尾皆有提供詳細的驗證步驟，可以確認代理式流量依正確方式路由，而且真人流量並未受到影響。

## 機會

下表顯示可改善代理式網頁體驗而且可由邊緣最佳化支援的機會。

| 機會 | 類型 | 自動識別 | 自動建議 | 自動最佳化 |
|---------|----------|----------|----------|----------|
| [復原內容能見度](/help/dashboards/opportunities/recover-content-visibility.md) | 技術性 GEO | 偵測對 AI 代理隱藏重要內容的頁面。 顯示受影響的 URL 和可復原的預期內容。 | 標示出指出可供 AI 代理使用的內容，並建議讓這些頁面進行預先轉譯。 | 將完整轉譯、適合 AI 使用的 HTML 快照提供給代理式流量，復原先前隱藏的內容。 |
| [擴充產品詳細資訊頁面](/help/dashboards/opportunities/enrich-product-detail-pages.md) | 技術性 GEO | 針對 Adobe Commerce 店面，比較完整目錄資料與 AI 代理在各產品詳細資訊頁面中可存取的內容；並顯示 AI 代理可見 HTML 中缺少變體、規格、屬性及相關目錄欄位的 PDP，並依代理式流量排定優先順序。 | 強調 AI 代理視圖中缺少且可復原的目錄資訊，以及這些資訊對 LLM 驅動產品搜尋的重要性。 | 在內容傳遞網路邊緣向代理式流量提供完整預先轉譯、適合 AI 使用的 HTML 快照，讓 AI 代理無需變更 CMS 或目錄，即可取得來自您目錄的豐富產品資訊。 |
| [新增適合 LLM 讀取的摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md) | 內容最佳化 | 識別因為在頁面或區段層級缺乏簡潔摘要與結構化重點，導致 AI 代理難以掃描及解讀的高流量頁面。 | 建議以現有內容為基礎、由 AI 生成的簡短摘要與重點內容。 | 將摘要與重點內容插入相關的 HTML 區段，改善模型解讀和說明頁面內容的方式。 |
| [新增相關常見問題集](/help/dashboards/opportunities/add-relevant-faqs.md) | 內容最佳化 | 識別因缺少與提示集相符的結構化常見問題集內容，導致 AI 代理難以將使用者問題對應至相關頁面的高流量頁面。 | 根據使用者意圖和現有頁面主題，建議 AI 生成的常見問題集內容。 | 將常見問題集內容注入 HTML，讓 AI 驅動的回答更容易搜尋到相關頁面，也提高頁面內容的相關性。 |
| [簡化複雜的內容](/help/dashboards/opportunities/simplify-complex-content.md) | 內容最佳化 | 標示含有複雜文字並可能妨礙 AI 理解的頁面。 | 提供複雜文字的 AI 生成簡化版本，同時保留原意。 | 重寫頁面中的複雜區段，以改善 AI 可讀性。 |
| [新增目錄](/help/dashboards/opportunities/add-table-of-contents.md) | 技術性 GEO | 偵測因為缺少清晰結構或導覽標題，導致 AI 代理難以剖析內容並將內容對應至使用者查詢的頁面。 | 建議新增結構化目錄，其中包含反映頁面主要區段的錨點連結標題。 | 將目錄插入 HTML 中，改善頁面結構，讓 AI 模型可以更輕鬆地擷取、對應和引用相關區段。 |
| [新增多媒體逐字稿摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md) | 內容最佳化 | 識別將關鍵資訊嵌入影片或其他媒體內，但因缺少機器可讀逐字稿或摘要，導致 AI 代理難以使用該內容的頁面。 顯示受影響的 URL 和建議的文字。 | 建議以媒體與頁面內容為基礎、由 AI 生成的逐字稿摘要。 | 將逐字稿摘要插入 HTML 中，讓代理式流量接收機器可讀文字 (例如插入於相關影片附近)。 |

### 其他工具

[AI 內容能見度檢查程式](https://chromewebstore.google.com/detail/ai-content-visibility-che/jbjngahjjdgonbeinjlepfamjdmdcbcc)瀏覽器擴充功能，會顯示 LLM 可以存取多少網頁內容以及哪些內容仍然隱藏。 這是一項免費使用的獨立診斷工具，不需要產品授權或設定。

只需點按一次，您便可以評估任何網站的機器可讀性。 您可以透過並排比較查看 AI 代理與真人使用者看到的內容，並預估使用 LLM Optimizer 可以復原多少內容。 請參閱 [AI 可以讀取您的網站嗎？](https://business.adobe.com/tw/blog/introducing-the-llm-optimizer-chrome-extension) 頁面取得更多資訊。

## 機會的詳細說明

在接下來的區段中，您可以查看邊緣最佳化支援的每個機會的其他詳細資料。

### 復原內容能見度

這個機會將標示那些因為用戶端轉譯，導致主要內容被隱藏使得 AI 代理無法讀取的頁面。 對於所指出的每個頁面，此機會將準確地告訴您 AI 代理視圖中缺少哪些內容、特別標示能見度缺口，並讓您直接套用變更以復原隱藏的內容。 當您透過邊緣最佳化部署此機會時，系統會將預先轉譯的 AI 最佳化頁面版本提供給 LLM 使用者代理，讓他們無需執行 Javascript 亦可存取完整內容。
這樣做可確保 AI 代理優先完整讀取頁面內容。 在該預先轉譯的 HTML 上，再套用其他增強功能。

>[!IMPORTANT]
>若使用邊緣最佳化部署，則此預先轉譯功能會自動套用至下列所有機會，以確保 AI 代理可以完整讀取頁面內容。

請參閱[復原內容能見度](/help/dashboards/opportunities/recover-content-visibility.md)，以取得儀表板操作示範、部署步驟和常見問題集。

### 擴充產品詳細資訊頁面

此機會鎖定那些購物者可透過互動式店面體驗查看完整產品資訊，但 AI 代理僅能取得內容有限的 HTML 快照之 Adobe Commerce 產品詳細資訊頁面。 目錄代理會比較您的權威 Commerce 目錄與 AI 代理可看見的 PDP，列出所有重要落差 (例如在靜態 HTML 中從未出現的產品變體或規格)，並讓您部署僅供機器人使用的邊緣回答，在不變更目錄記錄或真人 UI 的情況下，為 LLM 爬蟲恢復對等的資訊存取權。

請參閱[擴充產品詳細資訊頁面](/help/dashboards/opportunities/enrich-product-detail-pages.md)，以取得儀表板操作示範、部署步驟和常見問題集。

### 新增適合 LLM 的摘要

此機會識別藉由簡潔摘要與結構化重點，能讓 LLM 快速理解頁面上相關聲明並因而獲益的高流量頁面。 針對每個頁面，系統會偵測最需要摘要的位置，並根據現有內容在頁面或區段層級提供 AI 生成的摘要 (以及重點內容，若相關)。 當您透過邊緣最佳化進行部署時，該內容會插入 AI 代理擷取的 HTML 中，藉此提升 AI 回答中品牌呈現的準確性。

請參閱[新增適合 LLM 讀取的摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md)，查看更多有關此機會的詳細資訊。

### 新增相關常見問答集

此機會標記那些若有更多問答內容，將能夠在 AI 驅動的搜尋中更加貼合使用者意圖和提示的高流量頁面。 此機會針對每個頁面，根據提示集和頁面上的內容，提議 AI 生成的常見問題集區塊。 透過邊緣最佳化，這些常見問題集會插入到 HTML 中，讓您的頁面更加適合 AI 讀取，提高 AI 回答直接反映您的指引資訊之機率。

請參閱[新增相關常見問題集](/help/dashboards/opportunities/add-relevant-faqs.md)，以取得儀表板操作示範、部署步驟和常見問題集。

### 簡化複雜的內容

此機會將尋找具有複雜的長段落的頁面，因為這些段落可能降低 AI 理解能力。 對於超出可讀性臨界值的每個頁面，此機會將建立 AI 生成的內容，不但保留原意，而且變得更簡單，也更容易掃描。 當部署於邊緣時，傳遞至代理式流量的簡化內容將協助 LLM 更加忠實地解讀並總結您的內容。

請參閱[簡化複雜的內容](/help/dashboards/opportunities/simplify-complex-content.md)，以取得儀表板操作示範、部署步驟和常見問題集。

### 新增目錄

此機會偵測因標題與區段結構不清楚或缺乏，導致 AI 代理難以導覽的頁面。 對於每個受影響的頁面，此機會建議一套結構化目錄，其中含有與主要區段保持一致的錨點連結項目。 當您透過邊緣最佳化進行部署時，該目錄會插入 HTML 中，讓模型能更可靠地將使用者查詢對應至頁面中的正確區段並加以引用。

請參閱[新增目錄](/help/dashboards/opportunities/add-table-of-contents.md)，以取得儀表板操作示範、部署步驟和搶先體驗指引。

### 新增多媒體逐字稿摘要

此機會鎖定那些重要資訊僅位於影片播放內容中，且缺乏 AI 代理可讀取的逐字稿或文字摘要的頁面。 針對每個頁面，系統會建議 AI 生成的逐字稿，以及根據媒體內容重點產生的簡短摘要。 透過邊緣最佳化，這些摘要會以機器可讀文字的形式新增至 HTML 中，讓 AI 代理也能取得真人訪客觀看影片時獲得的相同內容。

請參閱[新增多媒體逐字稿摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md)，以取得儀表板操作示範、部署步驟和常見問題集。

## 在邊緣自動最佳化

對於每個機會，您可以預覽、編輯、部署、即時檢視和回復在邊緣的最佳化。

>[!VIDEO](https://video.tv.adobe.com/v/3477995/?captions=chi_hant&learn=on&enablevpops)

### 預覽

透過&#x200B;**預覽**，您可以在某項建議正式上線前了解其影響。 預覽會以並排方式，比較目前頁面與套用建議之後預期的 AI 最佳化版本之間的差異。 此視圖使用與驅動即時流量相同的邊緣最佳化邏輯，只是採用隔離的預覽模式。 這不會影響即時流量，因為這是供審閱使用的唯讀模擬。

![預覽](/help/assets/optimize-at-edge/preview.png)

### 編輯

透過&#x200B;**編輯**，您可以微調或重寫自動生成的建議，然後再進行部署。 您不必接受建議，反而可以透過編輯工作流程完全掌控相關建議。 該視圖在結構化的編輯器中顯示提議的變更，而您可以在編輯器中修改文字，以便更加貼合您原來的意圖。 在部署之後，經編輯的版本便會提供給 AI 代理。

![編輯](/help/assets/optimize-at-edge/edit.png)

### 部署

**部署**&#x200B;功能會發佈所選的建議，以便從邊緣將最佳化體驗提供給 AI 代理。 如果內容傳遞網路已完全路由，則網域中所有頁面通常會在數分鐘內將新變更上線。 如果僅針對特定路徑設定路由，則只有列入允許清單的頁面會將最佳化內容上線。

![部署](/help/assets/optimize-at-edge/deploy.png)

### 即時檢視

透過&#x200B;**即時檢視**，您可以驗證最佳化是否已上線而且針對代理式流量的運作符合預期，此一視圖難以透過其他方式取得。 您可以在「固定建議」下檢視即時頁面，而「固定建議」會按照向 AI 代理呈現的方式轉譯頁面。

![即時檢視](/help/assets/optimize-at-edge/view-live.png)

### 復原

復原可安全地回復先前部署的最佳化。 AI 限用的頁面版本通常會在數分鐘內恢復其先前的狀態，因此可以安心地在需要時進行最佳化實驗。

![復原](/help/assets/optimize-at-edge/rollback.png)

## 其他資源

如需更多有關「邊緣最佳化」功能的詳細資訊，請參閱下列播放清單 [LLM Optimizer — 邊緣最佳化](https://www.youtube.com/playlist?list=PLzbVcr6JHocVSMWBCaCw4xxjQ_VFVvFh0)。

## 常見問題

問：試用版客戶可以試用邊緣架構最佳化嗎？

可以，試用版客戶可以存取一個最佳化機會，並將其部署至最多 10 個頁面。 預設情況下，該機會是「復原內容能見度」，可讓 AI 代理存取頁面內容的完整版本。

問：您使用邊緣最佳化鎖定哪類 LLM？

要鎖定的使用者代理清單是由您在上線流程中定義的。

<!--
Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.
-->

問：如果我尚未開始使用邊緣最佳化，會發生什麼事？

如果您在完成必要的設定之前點按「**部署最佳化**」，您的網站將不會套用任何內容。 反而會出現快顯對話方塊，提示您透過 `llmo-at-edge@adobe.com` 聯絡我們的團隊以尋求上線方面的協助。 在上線完成之前，您仍然可以查看偵測到的機會和建議，但是依舊未能啟用一鍵部署的工作流程。

問：當來源更新內容時會發生什麼事？

只要基礎的來源頁面未變更，我們會從快取提供頁面的最佳化版本。 但是，當來源的&#x200B;**復原內容能見度**&#x200B;確實有所變更時，我們的系統會自動重新整理，讓 AI 代理總是收到最新的內容。 這是因為我們使用低快取存留時間 (TTL) 設定 (通常只有數分鐘)，以便您的網站上任何內容更新都會在該視窗中觸發新的最佳化。 針對&#x200B;**新增 LLM 友善摘要**&#x200B;等內容機會，LLM Optimizer 會監控來源頁面是否有所變更。 如果偵測到變更，我們會暫停最佳化並加上人工審閱的標記，防止 AI 代理可見頁面和真人可見頁面之間出現內容偏差。
<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

問：邊緣最佳化是否僅適用於使用 Adobe Edge Delivery Service (EDS) 的網站？

不是。 邊緣最佳化適用於任何內容傳遞網路，且可以搭配任何前端架構使用，而不限於部署在 Adobe EDS 堆疊上者。

問：邊緣最佳化的預先轉譯，與傳統的伺服器端轉譯 (SSR) 有何不同？

兩者各解決不同的問題，但可以搭配運作。 傳統 SSR 會轉譯伺服器端的內容，但不包含後來在瀏覽器中載入的內容。 邊緣最佳化預先轉譯在 JavaScript 和用戶端資料載入後擷取頁面，在內容傳遞網路邊緣產生完全組裝的版本。 SSR 著重於改善真人使用體驗，而邊緣最佳化則是改善 LLM 的網頁體驗。

問：復原內容能見度 (即預先轉譯) 是否屬於內容偽裝 (cloaking)？ 這聽起來像是向 AI 代理提供不同版本的頁面。

不是。 預先轉譯可確保 AI 代理能看見真人訪客與搜尋引擎最佳化爬蟲已能看到的相同內容。 許多網站會透過 JavaScript 載入重要內容，但典型 AI 代理並不會執行 JavaScript，因此可能會遺漏頁面中的大部分內容。 預先轉譯會產生擷取完整文字內容的靜態快照，讓 AI 代理取得與真人訪客及搜尋引擎相同的資訊。 這個功能會&#x200B;**復原** LLM 存取內容的對等權限；不會新增或變更實際內容。

問：那麼其他內容機會呢？例如「新增適合 LLM 讀取的摘要」，會在提供給 AI 代理的頁面中加入新的文字內容？ 這是內容偽裝嗎？

不是。 邊緣最佳化不會加入真人使用者與搜尋引擎最佳化爬蟲無法存取的資訊。 此服務會重新整理或摘要頁面上已有的內容，讓 AI 代理更輕鬆地解讀。 當使用者從 AI 回答中的連結前往您的網站時，仍可在實際頁面上找到相同的基礎資訊。

問：如果我只針對網域中的部分 URL (而非全部) 部署最佳化，會發生什麼事？

唯有您明確最佳化的 URL 會被修改。 對於已部署機會的 URL，AI 代理會收到最佳化版本。 對於並未部署任何機會的 URL，我們的服務只會依原樣代理原始頁面，而不會套用變更或將其儲存在最佳化快取層中。 這樣的做法讓您可以選擇性部署最佳化，而不會影響網站的其餘部分。
