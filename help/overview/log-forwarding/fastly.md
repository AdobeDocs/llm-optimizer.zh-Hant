---
title: 記錄轉送 - Fastly
description: 了解如何將內容傳遞網路記錄從 Fastly 轉送至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
autotag-review: '2026-07-15T17:51:11.580Z'
TQID: 'https://experienceleague.adobe.com/HPUxzfbvA4DtdNmjgTMvVVv-WqtjPcAUeZmg8JdvL-s'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
  - id: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
  - id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 381
ht-degree: 88%

---


# 記錄轉送：Fastly {#log-forwarding-fastly}

本頁面說明如何將內容傳遞網路記錄從 Fastly 轉送至 Adobe 的 S3 貯體，以收集代理式流量資料。 您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。 完成上線流程後，請依照本頁面提供的步驟，在 Fastly 網頁控制台中設定記錄轉送。

## 步驟 1：在 LLM Optimizer 上線 {#step-1}

在 LLM Optimizer 頁面 [https://llmo.now/](https://llmo.now/)：

1. 前往「**設定**」。

   ![「設定」按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**內容傳遞網路設定**」分頁。

   ![「內容傳遞網路設定」分頁](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下「**開始使用**」。
1. 在「**啟用 AI 流量洞察**」旁邊，按一下「**設定**」。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)
1. 選取「**Fastly (BYOCDN)**」。

   ![選取「Fastly」](/help/overview/assets/log-forwarding/fastly/fastly-select.png)
1. 按一下「**上線**」。

## 步驟 2：在 Fastly 中建立 S3 端點 {#step-2}

若要建立 S3 端點，請在 **Fastly 控制面板**&#x200B;上：

1. 在 Fastly 儀表板中，前往&#x200B;**內容傳遞網路服務** (不是運算服務)。
1. 在「**Amazon Web Services S3**」區域中，按一下「**建立端點**」。
1. 填寫「**建立 Amazon S3 端點**」欄位：

| 欄位 | 說明 |
| --- | --- |
| **名稱** | 人類可辨讀的端點名稱。 |
| **放置環境** | 預設 |
| **記錄格式** | 使用以下&#x200B;**記錄格式字串**&#x200B;區段中顯示的記錄格式字串。 |
| **時間戳記格式** | `%Y-%m-%dT%H:%M:%S.000` |
| **貯體名稱** | 從LLM Optimizer設定頁面複製&#x200B;**儲存貯體名稱**。 ![貯體名稱](/help/overview/assets/log-forwarding/fastly/fastly-bucket-name.png) |
| **網域** | 從LLM Optimizer設定頁面複製&#x200B;**網域名稱**。 ![網域名稱](/help/overview/assets/log-forwarding/fastly/fastly-domain-name.png) |
| **存取方法** | **使用者認證** |
| **使用者認證** | 從LLM Optimizer設定頁面複製&#x200B;**存取金鑰**&#x200B;和&#x200B;**秘密金鑰**。 ![存取金鑰](/help/overview/assets/log-forwarding/common/access-keys.png) |
| **時段** | `300` |

**記錄格式字串：**

```json
{ "timestamp": "%{strftime(\{"%Y-%m-%dT%H:%M:%S%z"\}, time.start)}V", "host": "%{if(req.http.Fastly-Orig-Host, req.http.Fastly-Orig-Host, req.http.Host)}V", "url": "%{json.escape(req.url)}V", "request_method": "%{json.escape(req.method)}V", "request_referer": "%{json.escape(req.http.referer)}V", "request_user_agent": "%{json.escape(req.http.User-Agent)}V", "response_status": %{resp.status}V, "response_content_type": "%{json.escape(resp.http.Content-Type)}V", "client_country_code": "%{client.geo.country_name}V", "time_to_first_byte": "%{time.to_first_byte}V" }
```

>[!WARNING]
>
>密碼管理員可能會使用您的 Fastly 密碼自動填入「**秘密金鑰**」欄位。 如果 AWS 整合失敗，請手動輸入秘密金鑰。

完成上述步驟後，按一下「**進階選項**」並設定：

| 欄位 | 說明 |
| --- | --- |
| **路徑** | 從LLM Optimizer設定頁面複製&#x200B;**路徑**。 ![路徑](/help/overview/assets/log-forwarding/fastly/fastly-path.png) |
| **選取記錄行格式** | 空白 |
| **壓縮** | Gzip |
| **備援層級** | 標準 |
| **ACL** | 無 |
| **伺服器端加密** | 無 |
| **最大位元組** | 0 |

設定進階選項後：

1. 按一下「**建立**」以建立端點。
1. 在「**啟動**」選單中選取「**在生產環境中啟動**」，以便進行部署。

>[!NOTE]
>
>Fastly 會將記錄持續串流到 S3，S3 網站和 API 僅在上傳完成後才提供檔案。

### 範例記錄項目 {#example}

以下是傳送資料至 Amazon S3 的格式字串範例：

```json
{
  "timestamp": "2026-02-10T05:05:36+0000",
  "host": "example.com",
  "url": "/my/path",
  "request_method": "GET",
  "request_referer": "https://example.com/my/other/path",
  "request_user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1",
  "response_status": 200,
  "response_content_type": "text/css; charset=utf-8",
  "client_country_code": "argentina",
  "time_to_first_byte": "0.138"
}
```
