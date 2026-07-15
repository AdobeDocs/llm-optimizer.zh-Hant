---
title: 記錄轉送 - Cloudflare
description: 了解如何將內容傳遞網路記錄從 Cloudflare 轉送至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
autotag-review: '2026-07-15T17:45:52.167Z'
TQID: 'https://experienceleague.adobe.com/6D7Xe-ysQOSsNMONyart2HaTfdyQR64l0r3AeFNsOpI'
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
ht-degree: 100%

---


# 記錄轉送：Cloudflare {#log-forwarding-cloudflare}

本頁面詳細說明如何將內容傳遞網路記錄從 Cloudflare 轉送至 Adobe 的 S3 貯體，以收集代理式流量資料。 您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。 完成上線流程後，請依照本頁面提供的步驟，在 Cloudflare 儀表板控制台中設定記錄轉送。

## 步驟 1：在 LLM Optimizer 上線 {#step-1}

在 LLM Optimizer 頁面 [https://llmo.now/](https://llmo.now/)：

1. 前往「**客戶設定儀表板**」。

   ![「設定」按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**內容傳遞網路設定**」分頁。

   ![「內容傳遞網路設定」分頁](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下「**開始使用**」。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png) -->

1. 在「**啟用 AI 流量洞察**」旁邊，按一下「**設定**」。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 選取「**Cloudflare (BYOCDN)**」。

   ![選取「Cloudflare」](/help/overview/assets/log-forwarding/cloudflare/cloudflare-select.png)

1. 按一下「**上線**」。

   <!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步驟 2：在 Cloudflare 中建立 Logpush 工作 {#step-2}

在 [Cloudflare 儀表板](https://dash.cloudflare.com/login)上，按照下列步驟操作：

1. 前往「**網域 (區域)**」層級的 **Logpush** 頁面。
1. 選取「**建立 Logpush 工作**」。
1. 在「**選取目標**」中，選擇「**Amazon S3**」。
1. 輸入下列目標資訊：

   - **貯體**：S3 貯體名稱。 從 LLM Optimizer 內容傳遞網路設定頁面複製該值。

     ![貯體名稱](/help/overview/assets/log-forwarding/common/bucket-name.png)

   - **路徑**：儲存體容器中的貯體位置。 從 LLM Optimizer 內容傳遞網路設定頁面複製該值。

     ![Cloudflare 路徑](/help/overview/assets/log-forwarding/cloudflare/cloudflare-path.png)

   - **整理記錄並存放至按日建立的子資料夾** (建議使用)。

     ![按日建立的子資料夾](/help/overview/assets/log-forwarding/cloudflare/cloudflare-daily-subfolders.png)

   - **貯體區域**：從 LLM Optimizer 內容傳遞網路設定頁面複製該值。

     <!-- ![Region](/help/overview/assets/log-forwarding/cloudflare/cloudflare-region.png)-->

   - 如果不需要伺服器端加密，請不要勾選。

   完成上述步驟後，請選取「**繼續**」。

1. 為了證明所有權，Cloudflare 會將一個檔案傳送至您指定的目標。 若要尋找權杖，請在所有權挑戰檔案的「**概觀**」分頁中按一下「**開啟**」按鈕。 從 LLM Optimizer 內容傳遞網路設定頁面複製所有權權杖，然後貼到 Cloudflare 儀表板中，驗證您對貯體的存取權。 輸入所有權權杖並選取「**繼續**」。

   <!--![Ownership token](/help/overview/assets/log-forwarding/cloudflare/cloudflare-ownership-token.png)-->

1. 選取「**HTTP 要求**」資料集，以推播至儲存服務。

1. 設定您的 Logpush 工作：

   - 輸入&#x200B;**工作名稱**。

   - 在「**傳送下列欄位**」中，檢視 LLM Optimizer 設定頁面中的值。

     ![Logpush 欄位](/help/overview/assets/log-forwarding/cloudflare/cloudflare-logpush-fields.png)

   - **記錄格式**：JSON。

     <!--![JSON format](/help/overview/assets/log-forwarding/cloudflare/cloudflare-json-format.png)-->

1. 在「**進階選項**」中：

   - 選擇記錄中的時間戳記欄位格式：`RFC3339`。

     ![時間戳記格式](/help/overview/assets/log-forwarding/cloudflare/cloudflare-timestamp-format.png)

   - 針對抽樣率，選取「**所有記錄**」。

     ![抽樣率](/help/overview/assets/log-forwarding/cloudflare/cloudflare-sampling-rate.png)

1. Logpush 工作設定完成後，請選取「**提交**」。
