---
title: 記錄轉送 - Akamai
description: 了解如何將內容傳遞網路記錄從 Akamai 轉送至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
autotag-review: '2026-07-15T17:40:31.728Z'
TQID: 'https://experienceleague.adobe.com/I-FHVzjiUvTwncGnfR2aAV2lOgtyltvac8aRN5LEQoE'
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
source-wordcount: 612
ht-degree: 78%

---


# 記錄轉送：Akamai {#log-forwarding-akamai}

本頁面說明如何將內容傳遞網路記錄從 Akamai 轉送至 Adobe 的 S3 貯體，以收集代理式流量資料。 您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。 上線流程完成後，請依照本頁面提供的步驟，在 Akamai 控制面板中設定記錄轉送。

## 步驟 1：在 LLM Optimizer 上線 {#step-1}

在 LLM Optimizer 頁面 [https://llmo.now/](https://llmo.now/)：

1. 前往「**客戶設定儀表板**」。

   ![「設定」按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**內容傳遞網路設定**」分頁。

   ![「內容傳遞網路設定」分頁](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下「**開始使用**」。

   <!--![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在「**啟用 AI 流量洞察**」旁邊，按一下「**設定**」。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 選取「**Akamai (BYOCDN)**」。

   ![選取「Akamai」](/help/overview/assets/log-forwarding/akamai/akamai-select.png)

1. 按一下「**上線**」。

   <!--![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步驟 2：在 Akamai 中建立串流 {#step-2}

在 Akamai 控制面板 [https://control.akamai.com/](https://control.akamai.com/) 上，依照官方 Akamai 文件中的步驟來[建立串流](https://techdocs.akamai.com/datastream2/docs/create-stream)。

## 步驟 3：選擇資料參數 {#step-3}

建立資料流後，請在Akamai控制檯上按一下&#x200B;**下一步**&#x200B;以繼續&#x200B;**資料集**&#x200B;索引標籤。 依照官方 Akamai 文件中的步驟，選擇[資料參數](https://techdocs.akamai.com/datastream2/docs/choose-data-parameters)。 將會需要下列來自 LLM Optimizer 設定的欄位：

![LLMO 設定欄位](/help/overview/assets/log-forwarding/akamai/akamai-llmo-config-fields.png)

對應關係應如下所示：

* **記錄檔資訊**
reqTimeSec ->要求時間
* **地理資料**
國家/地區 — >國家/地區
* **郵件交換資料**
reqHost ->要求主機
reqPath ->請求路徑
queryStr ->查詢字串（選擇性）
reqMethod -> Request方法
ua -> User-Agent
statusCode -> HTTP狀態代碼
rspContentType -> Response Content-Type
* **要求標頭資料**
referer -> Referer
* **網路效能資料**
timeToFirstByte ->到第一個位元組的時間

>[!NOTE]
>
>`queryStr`引數是選用的。 如果查詢字串包含PII資訊，則可以省略它。

Akamai 資料集欄位 (包括 ID) 如下所示：

1100，# reqTimeSec ->要求時間
2012，#國家 — >國家/地區
1011， # reqHost ->要求主機
1013， # reqPath ->請求路徑
2009，# queryStr ->查詢字串（選擇性）
1012，# reqMethod -> Request方法
1017， # ua -> User-Agent
1008， # statusCode -> HTTP狀態碼
1032， #推薦者 — >推薦者
1016， # rspContentType -> Response Content-Type
2025 # timeToFirstByte ->到第一個位元組的時間

## 步驟 4：設定目標 {#step-4}

建立資料串流並選擇參數後，您需要設定目標。 若要設定目標，請按照下列步驟操作：

1. 在「**目標**」中，選取「**S3**」。
2. 在「**名稱**」中，為目標輸入人類容易辨讀的描述。
3. 在「**貯體**」中，複製 LLM Optimizer 設定頁面中的「**貯體名稱**」。

   ![貯體名稱](/help/overview/assets/log-forwarding/common/bucket-name.png)

4. 在「**資料夾路徑**」中，複製 LLM Optimizer 設定頁面中的「**路徑**」。

   ![路徑設定](/help/overview/assets/log-forwarding/akamai/akamai-path-config.png)

5. 在「**地區**」中，複製 LLM Optimizer 設定頁面中的「**地區**」。

   <!--![Region](/help/overview/assets/log-forwarding/common/region.png)-->

6. 在「**存取金鑰 ID**」和「**祕密存取金鑰**」中，複製 LLM Optimizer 設定頁面中的這兩個值。

   ![存取金鑰](/help/overview/assets/log-forwarding/common/access-keys.png)

7. 按一下「**驗證並儲存**」以驗證與目標的連線，並儲存您提供的詳細資料。 在此驗證流程中，系統會使用所提供的存取金鑰識別碼和秘密存取金鑰，在 S3 資料夾中建立驗證檔案，並依照 `Akamai_access_verification_[TimeStamp].txt` 格式在檔案名稱中加入時間戳記。 只有在驗證流程成功，且您可以存取想傳送記錄過去的 Amazon S3 貯體和資料夾時，才能看到此檔案。

8. 在「**傳遞選項**」選單中，編輯「**檔案名稱**」欄位，如下所示：

   a. 變更&#x200B;**前置詞**。 在 LLM Optimizer 設定頁面，複製「**記錄檔案前置詞**」下的值：

   ```
   {%Y}-{%m}-{%d}T{%H}:{%M}:{%S}.000
   ```

   b. 變更&#x200B;**後綴**。 在 LLM Optimizer 設定頁面，複製「**記錄檔案後綴**」下的值。

9. 變更&#x200B;**推播頻率**。 在 LLM Optimizer 設定頁面，複製「**記錄間隔**」下的值。

   ![記錄間隔](/help/overview/assets/log-forwarding/akamai/akamai-log-interval.png)

10. 按一下「**下一步**」以完成流程。

在最終驗證前，設定應與以下範例所示類似：

![設定驗證](/help/overview/assets/log-forwarding/akamai/akamai-validation.png)
