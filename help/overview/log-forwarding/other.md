---
title: 記錄轉送 - 其他 (手動上傳)
description: 了解在使用不受支援的內容傳遞網路提供者時，如何手動將內容傳遞網路記錄上傳至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
autotag-review: '2026-05-15T17:54:15.685Z'
TQID: 'https://experienceleague.adobe.com/YBfhS4oM0qYRkFvS3zPzzcFAeLNBucRH5QmMBUH8h4E'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
topic_v2:
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 670
ht-degree: 100%

---


# 記錄轉送：其他 (手動上傳) {#log-forwarding-other}

**其他 BYOCDN** 佈建方法是一個涵蓋所有情況的選項，適用於要在下列情況下將內容傳遞網路記錄提供給 LLM Optimizer 的客戶：

- 建議使用&#x200B;**手動上傳**：例如，營運團隊會匯出記錄並定期上傳。
- 使用&#x200B;**臨時自動化流程**：一次性指令碼、排程匯出、無伺服器工作。
- 客戶所使用的&#x200B;**內容傳遞網路，是內建記錄轉送整合功能未原生支援的**。

此方法會模擬「持續轉送」模型：產生記錄並上傳至預期的 S3 位置，並且最終由攝取管道自動處理。

## 步驟 1：在 LLM Optimizer 上線 {#step-1}

在 [LLM Optimizer](https://llmo.now/) 上：

1. 前往「**設定**」。

   ![「設定」按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**內容傳遞網路設定**」分頁。

   ![「內容傳遞網路設定」分頁](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下「**開始使用**」。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在「**啟用 AI 流量洞察**」旁邊，按一下「**設定**」。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 選取「**其他**」。

   ![選取「其他」](/help/overview/assets/log-forwarding/other/other-select.png)

1. 按一下「**上線**」。

<!--   ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步驟 2：準備和上傳記錄 {#step-2}

### 必要的記錄格式 (JSON Lines) {#log-format}

記錄必須上傳為新行分隔的 JSON (**每行一個 JSON 物件**)。 每個記錄行都必須包含下列欄位，**拼法需與下列完全一致**。

#### 逐個欄位結構描述 {#schema}

| 欄位 | 類型 | 說明 | 範例 |
|---|---|---|---|
| **timestamp** | 字串 | 要求的時間戳記遵循 **ISO 8601** 格式。 | `"2025-02-01T23:00:05Z"` |
| **host** | 字串 | 用戶端要求的網頁網域。 | `"www.example.com"` |
| **url** | 字串 | 必須有&#x200B;**路徑**&#x200B;和&#x200B;**查詢參數**，但&#x200B;**不**&#x200B;應該包括網域。 | `"/home?utm_source=google"` |
| **request_method** | 字串 | HTTP 要求方法，有時稱為 HTTP 動詞。 | `"GET"` |
| **request_user_agent** | 字串 | HTTP User-Agent 要求標頭。 | `"Mozilla/5.0 (compatible; GPTBot/1.0"` |
| **request_referer** | 字串 | HTTP Referer 要求標頭 (可為空白)。 | `"https://chatgpt.com"` |
| **response_status** | 整數 | HTTP 回應狀態代碼。 | `200` |
| **response_content_type** | 字串 | HTTP Content-Type 回應標頭。 | `"text/html; charset=utf-8"` |
| **time_to_first_byte** | 整數 | 從建立伺服器連線到下載網頁內容之間的時間，以&#x200B;**毫秒**&#x200B;為單位。 如果未知或無法使用，則設為零。 | `42` |

#### 記錄行範例 {#example}

下列範例顯示三行記錄：

```json
{"timestamp":"2025-02-01T23:06:14Z","host":"www.example.com","url":"/products/llm-optimizer?utm_source=google","request_method":"GET","request_user_agent":"Mozilla/5.0 (compatible; GPTBot/1.0; +https://openai.com/gptbot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":198}
{"timestamp":"2025-02-01T23:19:32Z","host":"www.example.com","url":"/services/ai-consulting/overview","request_method":"GET","request_user_agent":"PerplexityBot/1.0 (+https://www.perplexity.ai/perplexitybot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":255}
{"timestamp":"2025-02-01T23:44:05Z","host":"www.example.com","url":"/products/pricing/enterprise?utm_medium=social","request_method":"GET","request_user_agent":"ClaudeBot/1.0 (+https://www.anthropic.com)","response_status":200,"request_referer":"","response_content_type":"application/pdf","time_to_first_byte":312}
```

### 重要免責聲明 (拼法和類型) {#disclaimer}

攝取和彙總管道對&#x200B;**欄位名稱和資料類型**&#x200B;有嚴格限制。

- 欄位名稱必須&#x200B;**完全**&#x200B;相符 (大小寫和拼法)。
- 資料類型必須正確，如下所示：
   - **timestamp** 必須是符合 **ISO 8601** 格式的字串。 類似 UNIX 的時間戳記可能無法運作。
   - **response_status** 必須是整數。
   - **time_to_first_byte** 必須為整數且使用毫秒為單位。
   - 字串必須是有效的 JSON 字串。
- 格式錯誤的 JSON 或遺失/不正確的欄位可能會導致略過記錄或無法剖析，導致報告中缺少資料。

### 上傳位置和處理頻率 {#upload-location}

#### 路徑規則 {#path-rule}

使用下列格式將記錄上傳至適當的資料夾路徑下：**`yyyy/mm/dd/`** (含斜線)。

2025 年 2 月 1 日 UTC 的記錄範例：`ABC123AdobeOrg/raw/byocdn-other/2025/02/01/`

#### 處理規則 {#processing-rule}

- 在特定 **UTC 日**&#x200B;以內上傳的記錄，會在&#x200B;**該 UTC 日接近結束時**&#x200B;由管道處理 (每日執行一次)。
- 上傳到&#x200B;**前幾天的資料夾** (回填) 的記錄會在 24 小時內被&#x200B;**偵測並處理**。

## 情境 {#scenarios}

### 情境 1：在 Splunk / Elasticsearch 中的記錄：匯出並上傳至 S3 {#scenario-splunk}

**目標**：從現有可觀察性平台檢索記錄，並將其傳遞到 S3 位置。

- 從 Splunk/Elastic 搜尋事件擷取所需欄位。
- 依照上述結構描述，將每個事件轉換為一個 JSON 物件 (JSON Lines)。
- 將產生的檔案上傳到指定的 S3 貯體和&#x200B;**目前 UTC 日**&#x200B;路徑：`…/byocdn-other/yyyy/mm/dd/`
- 記錄將在 UTC 日結束時自動處理。

### 情境 2：Lambda / Azure Function：格式化並上傳至 S3 {#scenario-serverless}

**目標**：使用無伺服器運算來擷取/接收內容傳遞網路記錄、將其標準化，並傳遞至 S3 位置。

- 函數會從客戶的來源 (記錄存放區、佇列、Blob 儲存空間等) 檢索記錄。
- 函數將欄位對應到預期的結構描述，並輸出 **JSON Lines** 格式。
- 函數會將輸出上傳至：`…/byocdn-other/yyyy/mm/dd/`
- 記錄將在 UTC 日結束時自動處理。

## 快速檢查清單 {#checklist}

- **每行一個 JSON 物件** (JSON Lines)
- 依照指示的&#x200B;**確切欄位拼法**
- 正確的資料類型
- **time_to_first_byte** 以毫秒為單位 (整數)
- 上傳至適當的 UTC 資料夾：**byocdn-other** 下的 **yyyy/mm/dd/**
