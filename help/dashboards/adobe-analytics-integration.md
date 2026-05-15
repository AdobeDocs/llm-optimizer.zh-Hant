---
title: Adobe Analytics 整合
description: 了解如何連結 Adobe Analytics 與 LLM Optimizer，在轉介流量儀表板中測量 AI 驅動的探索、網站參與和業務成果。
feature: Referral Traffic
autotag-review: '2026-05-15T17:29:50.263Z'
TQID: 'https://experienceleague.adobe.com/Wo-7p-mNQRrpS3EhmnS38UF1-IcaKEJpUz5H3BwY8Yo'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2: id: e69d5a42-0217-4ca5-9396-a9a826a170da
topic_v2: id: c2be0313-b3ae-45e0-b454-d20bf54b23f2id: e1e0219c-f879-479f-8427-888ed2a6e9c2id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 879
ht-degree: 100%

---


# Adobe Analytics 整合

Adobe Analytics 整合會將 LLM Optimizer 與您組織的 Adobe Analytics 資料連結，讓您能夠測量 AI 驅動的探索如何轉化為實際的網站參與度和業務成果。 整合程序完成後，可於「**業務影響**」標籤下的「**轉介流量**」儀表板中使用資料。

藉由連結分析資料與 AI 能見度洞察，LLM Optimizer 能協助您追蹤：

* AI 轉介頁面上的使用者參與度。
* 與 AI 探索歷程相連結的轉換訊號。
* 生成式引擎最佳化的績效影響。

這項整合能消弭 AI 能見度衡量與業務績效分析之間的差距。 團隊現在不僅可以檢視品牌如何出現於 AI 回答中，還可以檢視使用者進入網站後所發生的情形。

## 可用性 {#availability}

>[!IMPORTANT]
>
>Adobe Analytics 整合包含在付費 LLM Optimizer 產品方案中。 使用免費試用版的客戶在升級至付費產品方案之前，無法連結 Adobe Analytics。

## 將 Adobe Analytics 連結至轉介流量儀表板 {#connect}

連結流程從「[轉介流量](/help/dashboards/referral-traffic.md)」儀表板開始，如下所示：

1. 開啟「[轉介流量](/help/dashboards/referral-traffic.md)」儀表板。 預設視圖為「**流量洞察**」。

   ![轉介流量儀表板、流量洞察標籤](/help/dashboards/assets/aa-integration-01-referral-traffic-traffic-insights.png)

1. 選取「**業務影響**」標籤。 若未連結分析提供者，會顯示橫幅：「**連結以檢視業務影響**」，以及「**連結 Analytics**」。

   ![業務影響標籤以及連結 Analytics](/help/dashboards/assets/aa-integration-02-business-impact-connect.png)

1. 選取「**連結 Analytics**」。 接著會開啟位在「**Analytics**」標籤的「[客戶設定](/help/dashboards/customer-configuration.md)」儀表板。

   ![客戶設定、Analytics 標籤](/help/dashboards/assets/aa-integration-03-analytics-tab.png)

1. 在「**認證**」下，輸入「**用戶端 ID**」和「**用戶端密碼**」，然後選取「**驗證並繼續**」。 請注意下列事項：

   * 只有在兩個欄位都填寫時才能使用「**驗證並繼續**」。
   * 成功驗證後，便會載入報告套裝。
   * 您所使用的「**用戶端 ID**」和「**用戶端密碼**」，必須來自具備所需報告套裝之存取權的[技術帳戶](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/)。

   ![Analytics 認證及驗證並繼續](/help/dashboards/assets/aa-integration-04-credentials.png)

1. 在「**設定**」下，選擇一個「**報告套裝**」。

   選取報告套裝後，LLM Optimizer 會載入該套裝可用的「**頁面 URL 維度**」選項。

   ![已選取報告套裝，維度載入中](/help/dashboards/assets/aa-integration-05-report-suite.png)

1. 選擇一個「**頁面 URL 維度**」(套裝特定的維度清單)，然後選取「**儲存並啟用**」。

   * 在選取報告套裝並載入維度之前，「**頁面 URL 維度**」會保持停用狀態。
   * 只有在您選取頁面 URL 維度後才能使用「**儲存並啟用**」。

   ![頁面 URL 維度及儲存並啟用](/help/dashboards/assets/aa-integration-06-page-url-dimension.png)

1. 儲存後，設定應該會顯示「**已連結**」狀態。 您可以使用「**前往轉介流量儀表板**」返回轉介流量儀表板。 在「**業務影響**」標籤上的「**轉介流量**」中，狀態應顯示為「**已連結 Adobe Analytics**」。

   ![設定顯示已連結 Adobe Analytics 及業務影響](/help/dashboards/assets/aa-integration-07-connected.png)

### 完成連結之後 {#after-connect}

* LLM Optimizer 會回填&#x200B;**過去四個完整日曆週**&#x200B;以及&#x200B;**目前日曆週至今**&#x200B;的資料。
* 回填之後，資料會&#x200B;**每日**&#x200B;重新整理，並提取&#x200B;**前一整日**&#x200B;的資料。

>[!NOTE]
>
>回填可能需要幾小時才能完成。

## 運作方式 {#how-it-works}

### 設定

在設定期間，您會定義 LLM Optimizer 在進行 Adobe Analytics 資料攝取時所使用的報告套裝和頁面維度。 頁面維度可以是標準 `variables/page` 對應或自訂 `eVar`，取決於您的報告套裝。

### LLM 流量辨識方式

透過 Adobe Analytics [反向連結類型 — 對話式 AI 工具](https://experienceleague.adobe.com/zh-hant/docs/analytics/components/dimensions/referrer-type#conversational-ai-tools)辨識 LLM 所產生的流量。

### 攝取的資料 {#data-ingested}

LLM Optimizer 已攝取下列資料：

**維度**

* 反向連結網域
* 反向連結類型
* 國家/地區
* 裝置類型
* 選取的頁面維度

**量度**

* 頁面檢視量
* 造訪量
* 訪客
* 登入次數
* 退出
* 跳出數
* 單頁造訪次數
* 逗留時間
* 購物車數
* 加入購物車次數
* 從購物車移除次數
* 購物車瀏覽次數
* 結帳
* 訂單
* 收入
* 單位數

### LLM Optimizer 如何使用此資料

此資料集支援 LLM Optimizer 獲取以下洞察：

* 頁面層級 LLM 流量績效。
* 跨 LLM 來源的反向連結績效。
* 區域和裝置層級的趨勢。
* 下游商業轉化成果。

## 常見問題 {#faq}

問：試用版期間是否能使用 Adobe Analytics 整合？

否。 整合僅適用於付費版 LLM Optimizer 的客戶。

問：會收集或儲存哪些資料？

請參閱上方[攝取的資料](#data-ingested)章節。 LLM Optimizer 將使用您組織授權之 Adobe Analytics API 的彙總量度，而非原始點擊層級的資料。

問：用何種方式攝取資料？

您組織會授權 LLM Optimizer 查詢 Adobe Analytics API。 您可以透過這些 API 使用與 LLM 來源一致的轉介流量。

問：資料重新整理的頻率為何？

資料將&#x200B;**每日**&#x200B;重新整理 (回填完成後前一整日)。

問：原始點擊層級的資料是否儲存在 LLM Optimizer 中？

不是。 了解流量模式和趨勢時僅使用&#x200B;**彙總**&#x200B;量度。

問：是否會儲存完整 URL、查詢字串或頁面內容？

可以攝取所選頁面維度使用的完整 URL；此整合不會攝取查詢字串和頁面內容。

問：是否會儲存使用者識別碼 (ECID、IP 位址、Cookie ID)？

不會。

問：資料會保留多久？

請留意，保留原則可能會有變動。 目前，資料會無限期儲存。

問：資料在傳輸中及靜態時是否會加密？

目前，資料在傳輸中會加密，但靜止時不會。 這個情況在未來更新時可能會改變。

問：是否會回填歷史資料？

是。 設定成功後，系統會回填過去四個完整日曆週和目前日曆週的資料。 另請參閱[連結之後](#after-connect)。
