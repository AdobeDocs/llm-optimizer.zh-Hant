---
title: Adobe Analytics 整合
description: 瞭解如何連結Adobe Analytics與LLM Optimizer，在引薦流量儀表板中測量AI驅動的探索、網站參與和業務成果。
feature: Referral Traffic
source-git-commit: e7c9bc1d40267dc92608baa005f85f4be21cfda1
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 4%

---


# Adobe Analytics 整合

Adobe Analytics整合將LLM Optimizer與您組織的Adobe Analytics資料連線在一起，讓您能夠測量AI驅動的探索如何轉化為實際的網站參與度和業務成果。 整合程式完成後，資料將可在&#x200B;**業務影響**&#x200B;標籤下的&#x200B;**引薦流量**&#x200B;儀表板中使用。

透過連結分析資料與AI可見性深入分析，LLM Optimizer可協助您追蹤：

* 使用者在AI參照頁面上的參與。
* 與AI探索歷程繫結的轉換訊號。
* 地理位置最佳化的效能影響。

此整合可搭配AI可見度測量與業務績效分析之間的橋樑。 團隊現在不僅可以檢視品牌在AI回應中的顯示位置，還可以檢視使用者到達後會發生什麼情況。

## 可用性 {#availability}

>[!IMPORTANT]
>
>Adobe Analytics整合包含在付費LLM Optimizer選件中。 使用免費試用版的客戶在升級至付費選件之前，無法連線Adobe Analytics。

## 將Adobe Analytics連線至引薦流量控制面板 {#connect}

連線流程從[引薦流量](/help/dashboards/referral-traffic.md)儀表板開始，如下所示：

1. 開啟[引薦流量](/help/dashboards/referral-traffic.md)儀表板。 預設檢視為&#x200B;**流量分析**。

   ![引薦流量儀表板，流量分析標籤](/help/dashboards/assets/aa-integration-01-referral-traffic-traffic-insights.png)

1. 選取「**業務影響**」標籤。 如果沒有連線分析提供者，則會顯示橫幅： **連線以檢視業務影響**，並具有&#x200B;**連線至Analytics**。

   ![連線至Analytics的商業影響標籤](/help/dashboards/assets/aa-integration-02-business-impact-connect.png)

1. 選取&#x200B;**連線至Analytics**。 這會在&#x200B;**Analytics**&#x200B;標籤上開啟[客戶設定](/help/dashboards/customer-configuration.md)儀表板。

   ![客戶組態，Analytics索引標籤](/help/dashboards/assets/aa-integration-03-analytics-tab.png)

1. 在&#x200B;**認證**&#x200B;下，輸入&#x200B;**使用者端識別碼**&#x200B;和&#x200B;**使用者端密碼**，然後選取&#x200B;**驗證並繼續**。 請注意下列事項：

   * **驗證並繼續**&#x200B;只有在兩個欄位都填寫時才可用。
   * 在成功驗證後，即會載入報表套裝。
   * 使用[技術帳戶](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/)的&#x200B;**使用者端識別碼**&#x200B;和&#x200B;**使用者端密碼**，該帳戶具有您所需報表套裝的存取權。

   ![Analytics認證並驗證與繼續](/help/dashboards/assets/aa-integration-04-credentials.png)

1. 在&#x200B;**組態**&#x200B;底下，選擇&#x200B;**報表套裝**。

   選取報表套裝時，LLM Optimizer會載入該套裝可用的&#x200B;**頁面URL Dimension**&#x200B;選項。

   ![已選取報表套裝且維度正在載入](/help/dashboards/assets/aa-integration-05-report-suite.png)

1. 選擇&#x200B;**頁面URL Dimension** （套裝特定的維度清單），然後選取&#x200B;**儲存並啟用**。

   * 在選取報表套裝並載入維度之前，**頁面URL Dimension**&#x200B;會保持停用狀態。
   * **儲存並啟用**&#x200B;只有在您選取頁面URL維度後才能使用。

   ![頁面URL維度與儲存並啟用](/help/dashboards/assets/aa-integration-06-page-url-dimension.png)

1. 儲存後，設定應該會顯示&#x200B;**已連線**&#x200B;狀態。 您可以使用&#x200B;**返回引薦流量儀表板**&#x200B;來返回引薦流量儀表板。 在&#x200B;**業務影響**&#x200B;標籤上的&#x200B;**引薦流量**&#x200B;中，狀態應該顯示為&#x200B;**已連線至Adobe Analytics**。

   ![已連線至設定和商業影響中的Adobe Analytics](/help/dashboards/assets/aa-integration-07-connected.png)

### 連線之後 {#after-connect}

* LLM Optimizer會回填&#x200B;**過去四個完整的行事曆周**&#x200B;以及&#x200B;**目前行事曆周至今**。
* 回填之後，資料會以&#x200B;**完整的前一天**&#x200B;的提取在&#x200B;**每日**&#x200B;重新整理。

>[!NOTE]
>
>回填可能需要幾個小時才能完成。

## 運作方式 {#how-it-works}

### 設定

在設定期間，您可以定義LLM Optimizer用於Adobe Analytics擷取的報表套裝和頁面維度。 頁面維度可以是標準`variables/page`對應或自訂`eVar`，具體取決於您的報表套裝。

### 如何識別LLM流量

使用Adobe Analytics [反向連結型別 — 對話式AI工具](https://experienceleague.adobe.com/zh-hant/docs/analytics/components/dimensions/referrer-type#conversational-ai-tools)來識別LLM產生的流量。

### 已擷取的資料 {#data-ingested}

LLM Optimizer已內嵌下列資料：

**維度**

* 反向連結網域
* 反向連結型別
* 國家/地區
* 裝置型別
* 選取的頁面維度

**個量度**

* 頁面檢視量
* 造訪量
* 訪客
* 登入次數
* 退出
* 跳出數
* 單頁造訪
* 逗留時間
* 購物車
* 購物車新增
* 購物車移除
* 購物車檢視
* 結帳
* 訂單
* 收入
* 件數

### LLM Optimizer如何使用此資料

此資料集提供LLM Optimizer的深入分析：

* 頁面層級LLM流量效能。
* 跨LLM來源的反向連結效能。
* 地區和裝置層級的趨勢。
* 下游商業結果。

## 常見問題 {#faq}

問：Adobe Analytics整合是否可在試用期間使用？

不是。 此整合僅適用於付費LLM Optimizer客戶。

問：會收集或儲存哪些資料？

請參閱上方的[擷取的資料](#data-ingested)章節。 LLM Optimizer可與來自您組織授權的Adobe Analytics API的彙總量度搭配使用，而非原始點選層級資料。

問：如何擷取資料？

貴組織授權LLM Optimizer查詢Adobe Analytics API。 與LLM來源一致的引薦流量會透過這些API使用。

問：資料重新整理的頻率為何？

**每日**&#x200B;重新整理資料（回填完成後的前一天已滿）。

問：原始點選層級資料是否儲存在LLM Optimizer中？

不是。 只有&#x200B;**彙總**&#x200B;的量度可用來瞭解流量模式和趨勢。

問：是否已儲存完整URL、查詢字串或頁面內容？

可以擷取用於所選頁面維度的完整URL；此整合不會擷取查詢字串和頁面內容。

問：是否儲存使用者識別碼（ECID、IP位址、Cookie ID）？

不是。

問：資料會保留多久？

請記住，保留原則可能會有變動。 目前，資料會無限期儲存。

問：資料在傳輸中及閒置時是否已加密？

目前，資料在傳輸中並非靜止時加密。 這在未來更新中可能會變更。

問：歷史資料是否已回填？

可以。 成功設定後，系統會回填過去四個完整的日曆週和目前的日曆週。 另請參閱[在您連線之後](#after-connect)。
