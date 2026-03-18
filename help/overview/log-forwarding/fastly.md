---
title: 記錄轉送 — Fastly
description: 瞭解如何將CDN記錄檔從Fastly轉送至Adobe的S3貯體，以便在LLM Optimizer中收集代理流量資料。
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 3%

---


# 記錄轉送： Fastly {#log-forwarding-fastly}

本頁面說明如何將CDN記錄檔從Fastly轉送至Adobe的S3儲存貯體，以進行代理流量資料收集。 您將使用LLM Optimizer CDN設定頁面加入LLM Optimizer。 上線流程完成後，請依照本頁提供的步驟在Fastly Web主控台中設定記錄轉送。

## 步驟1：在LLM Optimizer中加入 {#step-1}

在LLM Optimizer頁面[https://llmo.now/](https://llmo.now/)上：

1. 移至&#x200B;**組態**。

   ![設定按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**CDN組態**」標籤。

   ![CDN設定索引標籤](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下&#x200B;**開始使用**。
1. 在&#x200B;**啟用AI流量深入分析**&#x200B;旁邊，按一下&#x200B;**設定**。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)
1. 選取&#x200B;**Fastly (BYOCDN)**。

   ![選取Fastly](/help/overview/assets/log-forwarding/fastly/fastly-select.png)
1. 按一下&#x200B;**內建**。

## 步驟2：在Fastly中建立S3端點 {#step-2}

若要建立S3端點，請在&#x200B;**Fastly控制檯**&#x200B;上：

1. 在Fastly儀表板中，移至&#x200B;**CDN服務** （不是運算服務）。
1. 在&#x200B;**Amazon Web Services S3**&#x200B;區域中，按一下&#x200B;**建立端點**。
1. 填寫&#x200B;**建立Amazon S3端點**&#x200B;欄位：

| 欄位 | 說明 |
| --- | --- |
| **名稱** | 端點的人類可讀名稱。 |
| **位置** | 預設 |
| **記錄格式** | 使用以下&#x200B;**記錄格式字串**&#x200B;區段中顯示的記錄格式字串。 |
| **時間戳記格式** | `%Y-%m-%dT%H:%M:%S.000` |
| **儲存貯體名稱** | 從LLM Optimizer設定頁面複製&#x200B;**儲存貯體名稱**。![儲存貯體名稱](/help/overview/assets/log-forwarding/fastly/fastly-bucket-name.png) |
| **網域** | 從LLM Optimizer設定頁面複製&#x200B;**網域名稱**。![網域名稱](/help/overview/assets/log-forwarding/fastly/fastly-domain-name.png) |
| **存取方法** | **使用者認證** |
| **使用者認證** | 從LLM Optimizer設定頁面複製&#x200B;**存取金鑰**&#x200B;和&#x200B;**秘密金鑰**。![存取金鑰](/help/overview/assets/log-forwarding/common/access-keys.png) |
| **週期** | `300` |

**記錄格式字串：**

```json
{ "timestamp": "%{strftime(\{"%Y-%m-%dT%H:%M:%S%z"\}, time.start)}V", "host": "%{if(req.http.Fastly-Orig-Host, req.http.Fastly-Orig-Host, req.http.Host)}V", "url": "%{json.escape(req.url)}V", "request_method": "%{json.escape(req.method)}V", "request_referer": "%{json.escape(req.http.referer)}V", "request_user_agent": "%{json.escape(req.http.User-Agent)}V", "response_status": %{resp.status}V, "response_content_type": "%{json.escape(resp.http.Content-Type)}V", "client_country_code": "%{client.geo.country_name}V", "time_to_first_byte": "%{time.to_first_byte}V" }
```

>[!WARNING]
>
>密碼管理員可能會使用您的Fastly密碼自動填入&#x200B;**秘密金鑰**&#x200B;欄位。 如果AWS整合失敗，請手動輸入秘密金鑰。

完成上述步驟後，按一下&#x200B;**進階選項**&#x200B;並設定：

| 欄位 | 說明 |
| --- | --- |
| **路徑** | 從LLM Optimizer設定頁面複製&#x200B;**路徑**。![路徑](/help/overview/assets/log-forwarding/fastly/fastly-path.png) |
| **選取日誌行格式** | 空白 |
| **壓縮** | Gzip |
| **備援等級** | 標準 |
| **ACL** | 無 |
| **伺服器端加密** | 無 |
| **最大位元組** | 0 |

設定進階選項後：

1. 按一下&#x200B;**建立**&#x200B;以建立端點。
1. 從&#x200B;**啟用**&#x200B;功能表，選取&#x200B;**在生產上啟用**&#x200B;以進行部署。

>[!NOTE]
>
>Fastly會將記錄持續串流到S3，S3網站和API僅在上傳完成後才提供檔案。

### 紀錄專案範例 {#example}

以下是傳送資料至Amazon S3的格式字串範例：

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
