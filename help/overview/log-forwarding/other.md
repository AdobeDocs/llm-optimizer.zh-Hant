---
title: 記錄轉送 — 其他（手動上傳）
description: 瞭解在使用不受支援的CDN提供者時，如何手動將CDN記錄檔上傳至Adobe的S3儲存貯體，以便在LLM Optimizer中進行一般流量資料收集。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 2%

---


# 記錄轉送：其他（手動上傳） {#log-forwarding-other}

**其他BYOCDN**&#x200B;布建方法是一個全包選項，適用於想在下列情況下將CDN記錄提供給LLM Optimizer的客戶：

- **建議使用手動上傳** — 例如，作業團隊會匯出記錄檔並定期上傳。
- **使用了臨機自動化程式** — 一次性指令碼、排程匯出、無伺服器工作。
- 客戶使用的&#x200B;**CDN不是由內建記錄轉送整合原生支援的**。

此方法會模擬「持續轉送」模型：產生記錄檔並上傳至預期的S3位置，最終由內嵌管道自動處理。

## 步驟1：在LLM Optimizer中加入 {#step-1}

在[LLM Optimizer](https://llmo.now/)：

1. 移至&#x200B;**組態**。

   ![設定按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**CDN組態**」標籤。

   ![CDN設定索引標籤](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下&#x200B;**開始使用**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**啟用AI流量深入分析**&#x200B;旁邊，按一下&#x200B;**設定**。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 選取&#x200B;**其他**。

   ![選取其他](/help/overview/assets/log-forwarding/other/other-select.png)

1. 按一下&#x200B;**內建**。

<!--   ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步驟2：準備和上傳記錄檔 {#step-2}

### 必要的記錄格式（JSON行） {#log-format}

記錄檔必須上傳為新行分隔的JSON （每行&#x200B;**一個JSON物件）。**&#x200B;每個記錄行都必須包含下列欄位&#x200B;**，其拼字完全低於**。

#### 依欄位結構描述 {#schema}

| 欄位 | 類型 | 說明 | 範例 |
|---|---|---|---|
| **timestamp** | 字串 | 要求的時間戳記遵循&#x200B;**ISO 8601**&#x200B;格式。 | `"2025-02-01T23:00:05Z"` |
| **主機** | 字串 | 使用者端要求的網頁網域。 | `"www.example.com"` |
| **url** | 字串 | 需要&#x200B;**路徑**&#x200B;和&#x200B;**查詢引數**，但網域應該&#x200B;**不包括**。 | `"/home?utm_source=google"` |
| **request_method** | 字串 | HTTP要求方法，有時稱為HTTP動詞。 | `"GET"` |
| **request_user_agent** | 字串 | HTTP User-Agent要求標頭。 | `"Mozilla/5.0 (compatible; GPTBot/1.0"` |
| **request_referer** | 字串 | HTTP Referer請求標頭（可為空白）。 | `"https://chatgpt.com"` |
| **response_status** | 整數 | HTTP回應狀態代碼。 | `200` |
| **response_content_type** | 字串 | HTTP Content-Type回應標頭。 | `"text/html; charset=utf-8"` |
| **time_to_first_byte** | 整數 | 從建立伺服器連線到下載網頁內容之間的時間是&#x200B;**毫秒**。 如果未知或無法使用，則設為零。 | `42` |

#### 紀錄行範例 {#example}

下列範例顯示三行記錄：

```json
{"timestamp":"2025-02-01T23:06:14Z","host":"www.example.com","url":"/products/llm-optimizer?utm_source=google","request_method":"GET","request_user_agent":"Mozilla/5.0 (compatible; GPTBot/1.0; +https://openai.com/gptbot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":198}
{"timestamp":"2025-02-01T23:19:32Z","host":"www.example.com","url":"/services/ai-consulting/overview","request_method":"GET","request_user_agent":"PerplexityBot/1.0 (+https://www.perplexity.ai/perplexitybot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":255}
{"timestamp":"2025-02-01T23:44:05Z","host":"www.example.com","url":"/products/pricing/enterprise?utm_medium=social","request_method":"GET","request_user_agent":"ClaudeBot/1.0 (+https://www.anthropic.com)","response_status":200,"request_referer":"","response_content_type":"application/pdf","time_to_first_byte":312}
```

### 嚴重免責宣告（拼字與型別） {#disclaimer}

擷取和彙總管道對&#x200B;**欄位名稱和資料型別**&#x200B;有嚴格限制。

- 欄位名稱必須與&#x200B;**完全相符** （大小寫與拼字）。
- 資料型別必須正確，如下所示：
   - **timestamp**&#x200B;必須是具有&#x200B;**ISO 8601**&#x200B;格式的字串。 類似UNIX的時間戳記可能無法運作。
   - **response_status**&#x200B;必須是整數。
   - **time_to_first_byte**&#x200B;必須為整數且使用毫秒。
   - 字串必須是有效的JSON字串。
- 格式錯誤的JSON或遺失/不正確的欄位可能會導致略過記錄或無法剖析，導致報表中缺少資料。

### 上傳位置和處理步調 {#upload-location}

#### 路徑規則 {#path-rule}

使用下列格式在適當的資料夾路徑下上傳記錄檔： **`yyyy/mm/dd/`** （含斜線）。

2025年2月1日UTC的紀錄範例： `ABC123AdobeOrg/raw/byocdn-other/2025/02/01/`

#### 處理規則 {#processing-rule}

- 在指定&#x200B;**UTC日**&#x200B;期間上傳的記錄檔會在該UTC日&#x200B;**接近結尾時（每日執行）由管道**&#x200B;處理。
- 上傳到&#x200B;**前幾天資料夾** （回填）的記錄檔會在24小時內偵測到&#x200B;**並處理**。

## 情境 {#scenarios}

### 案例1：在Splunk / Elasticsearch中登入 — 匯出並上傳至S3 {#scenario-splunk}

**目標**：從現有可觀察性平台擷取記錄檔，並將它們傳送到S3位置。

- 從Splunk/Elastic搜尋事件擷取必填欄位。
- 依照上述結構描述（JSON線），將每個事件轉換為一個JSON物件。
- 將產生的檔案上傳到指定的S3儲存貯體和&#x200B;**目前UTC日**&#x200B;路徑： `…/byocdn-other/yyyy/mm/dd/`
- 記錄檔將在UTC當天結束時自動處理。

### 案例2：Lambda / Azure函式 — 格式化並上傳至S3 {#scenario-serverless}

**目標**：使用無伺服器運算來擷取/接收CDN記錄檔、將其標準化，並傳送至S3位置。

- 函式會從客戶的來源（記錄存放區、佇列、Blob儲存等）擷取記錄。
- 此函式將欄位對應到預期的結構描述中，並發出&#x200B;**JSON行**。
- 函式會將輸出上傳至： `…/byocdn-other/yyyy/mm/dd/`
- 記錄檔將在UTC當天結束時自動處理。

## 快速檢查清單 {#checklist}

- **每行**&#x200B;一個JSON物件（JSON行）
- **指定的確切欄位拼字**
- 更正資料型別
- **time_to_first_byte** （以毫秒為單位，整數）
- 上傳至適當的UTC資料夾： **byocdn-other**&#x200B;下的&#x200B;**yyyy/mm/dd/**
