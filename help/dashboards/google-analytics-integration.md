---
title: Google Analytics整合
description: 瞭解如何將Google Analytics 4與LLM Optimizer連結，以在引薦流量儀表板中測量AI驅動的探索、網站參與和業務成果。
feature: Referral Traffic
source-git-commit: abf88fc3e141e12d6b5c826e35d4590ae6407c9b
workflow-type: tm+mt
source-wordcount: '1168'
ht-degree: 17%

---


# Google Analytics整合

Google Analytics 4 (GA4)整合將LLM Optimizer與您組織的GA4資料連線在一起，讓您能夠測量ChatGPT、Gemini、Copilot、Claude和Perplexity等平台上的AI驅動探索如何轉化為實際的網站參與度和業務成果。 連線GA4屬性後，LLM Optimizer會將GA4屬性的引薦流量、參與和轉換量度提取至這些來源，並將其顯示在「**業務影響**」標籤下的「**引薦流量**」儀表板中。

## 可用性 {#availability}

>[!IMPORTANT]
>
>GA4整合包含在付費LLM Optimizer選件中。 使用免費試用版的客戶在升級至付費選件之前，將無法連線GA4。

## 開始之前 {#before-you-begin}

若要完成連線，您需要：

* 對您要連線的GA4屬性具有至少&#x200B;**檢視器**&#x200B;存取權的Google帳戶。 在Google Analytics中的&#x200B;**管理員>屬性存取管理**&#x200B;下管理屬性層級存取。
* 在LLM Optimizer中管理設定的許可權（否則，可看見但停用連線按鈕）。
* 允許來自LLM Optimizer來源（Google登入步驟）的快顯視窗的瀏覽器會在新標籤中開啟。

您&#x200B;**不**&#x200B;需要建立Google雲端專案、產生服務帳戶、上傳JSON金鑰或輸入屬性ID。 LLM Optimizer會透過Google的標準OAuth同意畫面代理連線。

## 將GA4連線至引薦流量控制面板 {#connect}

連結流程從「[轉介流量](/help/dashboards/referral-traffic.md)」儀表板開始，如下所示：

1. 在LLM Optimizer中開啟&#x200B;**引薦流量**。

1. 開啟&#x200B;**業務影響**&#x200B;標籤。

   ![引薦流量儀表板，業務影響標籤](/help/dashboards/assets/ga4-integration-01-business-impact-tab.png)

1. 選取「**連結 Analytics**」。 LLM Optimizer會將您路由至&#x200B;**客戶設定> Analytics**。 在Analytics提供者選擇器中，選取&#x200B;**Google Analytics 4**。

   ![已選取GA4的客戶組態、Analytics索引標籤](/help/dashboards/assets/ga4-integration-02-analytics-ga4-picker.png)

1. 選取&#x200B;**連線**。 新瀏覽器標籤隨即開啟，顯示Google的登入畫面。

   用於GA4連線的![Google登入](/help/dashboards/assets/ga4-integration-03-google-sign-in.png)

1. 使用可存取您要連線之GA4屬性的Google帳戶登入。 在Google提示您時核准`See and download your Google Analytics data`許可權（`analytics.readonly`範圍）。

1. Google會帶您返回LLM Optimizer，其中列出您的帳戶可存取的GA4屬性。 選取要連線並提交的屬性。

1. 返回LLM Optimizer標籤。 Analytics索引標籤會自動偵測已完成的連線，且GA4卡會顯示&#x200B;**已連線**&#x200B;狀態。

### 完成連結之後 {#after-connect}

GA4連線至LLM Optimizer後，會發生下列情況：

* LLM Optimizer 會回填&#x200B;**過去四個完整日曆週**&#x200B;以及&#x200B;**目前日曆週至今**&#x200B;的資料。
* 回填之後，資料會&#x200B;**每日**&#x200B;重新整理，並提取&#x200B;**前一整日**&#x200B;的資料。

>[!NOTE]
>
>回填可能需要幾小時才能完成。 「業務影響」控制面板會隨著資料登陸而開始逐步填入；執行回填時，您不需要採取任何動作。

如果您重新連線（例如切換Google帳戶或GA4屬性），則只會再次回填目前的日曆週，而會保留已載入的前幾週。

## 運作方式 {#how-it-works}

### 連線模型

整合使用Google的標準OAuth 2.0使用者委派流程。 LLM Optimizer會儲存範圍設定在您所選GA4屬性的重新整理Token，該權杖可讓LLM Optimizer代表您呼叫GA4資料API （具有唯讀存取權），直到您從Google帳戶撤銷它為止。

### LLM 流量辨識方式

LLM Optimizer只要求GA4處理工作階段GA4本身歸因於LLM平台的作業。 今天，`sessionSourceMedium`符合`chatgpt`、`gemini.google.com`、`copilot.microsoft.com`、`claude`或`perplexity`其中一項的工作階段。 支援的LLM來源清單由Adobe維護，並可能會隨著時間而擴展。

### 攝取的資料 {#data-ingested}

每個每日提取都會擷取包含下列專案的彙總報表：

**維度**

| GA4維度 | 其代表內容 |
| --- | --- |
| `date` | 工作階段發生的日期。 |
| `landingPage` | 訪客在您網站上看到的第一個頁面。 |
| `countryId` | 訪客的國家/地區，由GA4的IP地理位置決定。 |
| `deviceCategory` | 行動裝置/桌上型電腦/平板電腦。 |
| `sessionSourceMedium` | GA4所歸因的LLM來源/媒體。 |

**量度**

| GA4量度 | 其代表內容 |
| --- | --- |
| `sessions` | 貯體中的工作階段數。 |
| `screenPageViews` | 貯體中的頁面檢視。 |
| `bounceRate` | 跳出的工作階段比例。 |
| `totalPurchasers` | 不同的購買者（如果在GA4中設定電子商務）。 |
| `transactions` | 交易計數（如果已設定電子商務）。 |
| `purchaseRevenue` | 購買收入(USD)。 |
| `totalRevenue` | 總收入（美元）。 |

### LLM Optimizer 如何使用此資料

LLM Optimizer會使用此資料來填入業務影響控制面板的頁面層級效能、來源劃分、國家/地區和裝置分割以及時間趨勢。 沒有資料可用於訓練模型或在您的租使用者外部共用。

### 未擷取的內容

無使用者識別碼（Google使用者端ID、IP位址、裝置ID）、無工作階段層級列、無事件層級列、無上述維度或量度以外的自訂維度或量度，且無GA4對象或區段定義。

## 常見問題 {#faq}

問：GA4整合是否可在試用期間使用？

否。 整合僅適用於付費版 LLM Optimizer 的客戶。

問：我需要建立Google Cloud專案或服務帳戶嗎？

不是。 連線是標準的Google登入。 LLM Optimizer在Adobe端管理Google OAuth使用者端；您只需要在GA4屬性上擁有檢視器存取權的Google帳戶。

問：會收集或儲存哪些資料？

LLM Optimizer可與來自您組織授權的GA4資料API的彙總量度搭配使用，而非原始事件層級資料。

問：用何種方式攝取資料？

貴組織授權LLM Optimizer查詢GA4資料API以取得選取的屬性。 會透過該API使用與LLM來源一致的引薦流量。

問：資料重新整理的頻率為何？

資料將&#x200B;**每日**&#x200B;重新整理 (回填完成後前一整日)。

問：原始事件層級資料是否儲存在LLM Optimizer中？

不是。 了解流量模式和趨勢時僅使用&#x200B;**彙總**&#x200B;量度。

問：是否會儲存完整 URL、查詢字串或頁面內容？

著陸頁面路徑是作為標準報表的一部分擷取；此整合不會擷取查詢字串和頁面內容。

問：是否已儲存使用者識別碼（Google使用者端ID、IP位址、裝置ID）？

不會。

問：資料會保留多久？

目前，資料會無限期儲存。

問：資料在傳輸中及靜態時是否會加密？

目前，資料在傳輸中而非靜止時加密。 這個情況在未來更新時可能會改變。

問：是否會回填歷史資料？

是。 設定成功後，系統會回填過去四個完整日曆週和目前日曆週的資料。 另請參閱[連結之後](#after-connect)。

問：我可以中斷連線或撤銷存取權嗎？

是 — 隨時。 您可以從LLM Optimizer中的GA4卡重新連線至其他帳戶或屬性，或完全從您位於[https://myaccount.google.com/permissions](https://myaccount.google.com/permissions)的Google帳戶撤銷LLM Optimizer的存取權。 撤銷存取權會停止提取新資料；先前擷取的彙總資料會保留在LLM Optimizer中。

問：我的GA4屬性已連線，但「業務影響」是空的 — 為什麼？

LLM Optimizer只會提取GA4本身歸因於受支援LLM來源/媒體的工作階段（今日：ChatGPT、Gemini、Copilot、Claude、Perplexity）。 如果您的GA4屬性尚未在顯示的時間範圍內記錄來自這些來源的反向連結工作階段，則控制面板將為空白，即使連線狀況良好。
