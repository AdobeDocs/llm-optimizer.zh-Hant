---
title: 記錄轉送 - Akamai
description: 了解如何將內容傳遞網路記錄從 Akamai 轉送至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: ht
source-wordcount: '595'
ht-degree: 100%

---


# 記錄轉送：Akamai {#log-forwarding-akamai}

本頁面說明如何將內容傳遞網路記錄從 Akamai 轉送至 Adobe 的 S3 貯體，以收集代理式流量資料。您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。上線流程完成後，請依照本頁面提供的步驟，在 Akamai 控制面板中設定記錄轉送。

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

建立串流後，在 Akamai 控制面板上，按一下「下一步」以繼續前往「**資料集**」分頁。依照官方 Akamai 文件中的步驟，選擇[資料參數](https://techdocs.akamai.com/datastream2/docs/choose-data-parameters)。將會需要下列來自 LLM Optimizer 設定的欄位：

![LLMO 設定欄位](/help/overview/assets/log-forwarding/akamai/akamai-llmo-config-fields.png)

對應關係應如下所示：

* **記錄資訊**
reqTimeSec -> 要求時間
* **地理資料**
country -> 國家/地區
* **訊息交換資料**
reqHost -> 要求主機
reqPath -> 要求路徑
queryStr -> 查詢字串
reqMethod -> 要求方法
ua -> User-Agent
statusCode -> HTTP 狀態代碼
rspContentType -> 回應內容類型
* **要求標頭資料**
referer -> 反向連結
* **網路效能資料**
timeToFirstByte -> 第一個位元組時間

Akamai 資料集欄位 (包括 ID) 如下所示：

1100，# reqTimeSec -> 要求時間
2012，# country -> 國家/地區
1011，# reqHost -> 要求主機
1013，# reqPath -> 要求路徑
2009，# queryStr -> 查詢字串
1012，# reqMethod -> 要求方法
1017，# ua -> 使用者代理
1008，# statusCode -> HTTP 狀態代碼
1032，# referer -> 反向連結
1016，# rspContentType -> 回應內容類型
2025，# timeToFirstByte -> 第一個位元組時間

## 步驟 4：設定目標 {#step-4}

建立資料串流並選擇參數後，您需要設定目標。若要設定目標，請按照下列步驟操作：

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

7. 按一下「**驗證並儲存**」以驗證與目標的連線，並儲存您提供的詳細資料。在此驗證流程中，系統會使用所提供的存取金鑰識別碼和秘密存取金鑰，在 S3 資料夾中建立驗證檔案，並依照 `Akamai_access_verification_[TimeStamp].txt` 格式在檔案名稱中加入時間戳記。只有在驗證流程成功，且您可以存取想傳送記錄過去的 Amazon S3 貯體和資料夾時，才能看到此檔案。

8. 在「**傳遞選項**」選單中，編輯「**檔案名稱**」欄位，如下所示：

   a. 變更&#x200B;**前置詞**。在 LLM Optimizer 設定頁面，複製「**記錄檔案前置詞**」下的值：

   ```
   {%Y}-{%m}-{%d}T{%H}:{%M}:{%S}.000
   ```

   b. 變更&#x200B;**後綴**。在 LLM Optimizer 設定頁面，複製「**記錄檔案後綴**」下的值。

9. 變更&#x200B;**推播頻率**。在 LLM Optimizer 設定頁面，複製「**記錄間隔**」下的值。

   ![記錄間隔](/help/overview/assets/log-forwarding/akamai/akamai-log-interval.png)

10. 按一下「**下一步**」以完成流程。

在最終驗證前，設定應與以下範例所示類似：

![設定驗證](/help/overview/assets/log-forwarding/akamai/akamai-validation.png)
