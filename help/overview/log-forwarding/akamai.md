---
title: 記錄轉送 — Akamai
description: 瞭解如何將CDN記錄檔從Akamai轉送至Adobe的S3貯體，以便在LLM Optimizer中收集代理流量資料。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---


# 記錄轉送： Akamai {#log-forwarding-akamai}

本頁面說明如何將CDN記錄檔從Akamai轉送至Adobe的S3儲存貯體，以進行代理流量資料收集。 您將使用LLM Optimizer CDN設定頁面加入LLM Optimizer。 上線流程完成後，請依照本頁提供的步驟，在Akamai控制面板中設定記錄轉送。

## 步驟1：在LLM Optimizer中加入 {#step-1}

在LLM Optimizer頁面[https://llmo.now/](https://llmo.now/)上：

1. 前往&#x200B;**客戶設定儀表板**。

   ![設定按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**CDN組態**」標籤。

   ![CDN設定索引標籤](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下&#x200B;**開始使用**。

   <!--![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**啟用AI流量深入分析**&#x200B;旁邊，按一下&#x200B;**設定**。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 選取&#x200B;**Akamai (BYOCDN)**。

   ![選取Akamai](/help/overview/assets/log-forwarding/akamai/akamai-select.png)

1. 按一下&#x200B;**內建**。

   <!--![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步驟2：在Akamai中建立資料流 {#step-2}

在Akamai控制面板[https://control.akamai.com/](https://control.akamai.com/)上，依照官方Akamai檔案中的步驟來[建立資料流](https://techdocs.akamai.com/datastream2/docs/create-stream)。

## 步驟3：選擇資料引數 {#step-3}

建立資料流後，在Akamai控制面板上，按一下「下一步」以繼續&#x200B;**資料集**&#x200B;索引標籤。 依照官方Akamai檔案中的步驟，選擇[資料引數](https://techdocs.akamai.com/datastream2/docs/choose-data-parameters)。 將需要LLM Optimizer設定的下列欄位：

![LLMO設定欄位](/help/overview/assets/log-forwarding/akamai/akamai-llmo-config-fields.png)

對應應如下所示：

* **記錄檔資訊**
reqTimeSec ->要求時間
* **地理資料**
國家/地區 — >國家/地區
* **郵件交換資料**
reqHost ->要求主機
reqPath ->請求路徑
queryStr ->查詢字串
reqMethod -> Request方法
ua -> User-Agent
statusCode -> HTTP狀態代碼
rspContentType -> Response Content-Type
* **要求標頭資料**
referer -> Referer
* **網路效能資料**
timeToFirstByte ->到第一個位元組的時間

Akamai資料集欄位（包括ID）如下：

1100，# reqTimeSec ->要求時間
2012，#國家 — >國家/地區
1011， # reqHost ->要求主機
1013， # reqPath ->請求路徑
2009， # queryStr ->查詢字串
1012，# reqMethod -> Request方法
1017， # ua -> User-Agent
1008， # statusCode -> HTTP狀態碼
1032， #推薦者 — >推薦者
1016， # rspContentType -> Response Content-Type
2025 # timeToFirstByte ->到第一個位元組的時間

## 步驟4：設定目的地 {#step-4}

建立資料串流並選擇設定目的地所需的引數後， 若要設定目的地，請執行下列步驟：

1. 在&#x200B;**目的地**&#x200B;中，選取&#x200B;**S3**。
2. 在&#x200B;**Name**&#x200B;中，輸入目的地的人類可讀描述。
3. 在&#x200B;**Bucket**&#x200B;中，從LLM Optimizer設定頁面複製&#x200B;**Bucket名稱**。

   ![儲存貯體名稱](/help/overview/assets/log-forwarding/common/bucket-name.png)

4. 在&#x200B;**資料夾路徑**&#x200B;中，從LLM Optimizer設定頁面複製&#x200B;**路徑**。

   ![路徑設定](/help/overview/assets/log-forwarding/akamai/akamai-path-config.png)

5. 在&#x200B;**地區**&#x200B;中，從LLM Optimizer設定頁面複製&#x200B;**地區**。

   <!--![Region](/help/overview/assets/log-forwarding/common/region.png)-->

6. 在&#x200B;**存取金鑰識別碼**&#x200B;和&#x200B;**機密存取金鑰**&#x200B;中，從LLM Optimizer設定頁面複製這兩個值。

   ![存取金鑰](/help/overview/assets/log-forwarding/common/access-keys.png)

7. 按一下「驗證並儲存」**&#x200B;**&#x200B;以驗證與目的地的連線，並儲存您提供的詳細資料。 在此驗證程式中，系統會使用提供的存取金鑰識別碼和機密存取金鑰，在S3資料夾中建立驗證檔案，且檔案名稱中的時間戳記為`Akamai_access_verification_[TimeStamp].txt`格式。 只有在驗證程式成功，且您可存取您嘗試將記錄傳送至的Amazon S3貯體和資料夾時，才能看到此檔案。

8. 在&#x200B;**傳遞選項**&#x200B;功能表中，編輯&#x200B;**檔案名稱**&#x200B;欄位，如下所示：

   答： 變更&#x200B;**首碼**。 從LLM Optimizer設定頁面複製&#x200B;**記錄檔首碼**&#x200B;下的值：

   ```
   {%Y}-{%m}-{%d}T{%H}:{%M}:{%S}.000
   ```

   b. 變更&#x200B;**尾碼**。 從&#x200B;**記錄檔尾碼**&#x200B;下的LLM Optimizer設定頁面複製值。

9. 變更&#x200B;**推播頻率**。 從&#x200B;**記錄間隔**&#x200B;下的LLM Optimizer設定頁面複製值。

   ![記錄間隔](/help/overview/assets/log-forwarding/akamai/akamai-log-interval.png)

10. 按一下&#x200B;**下一步**&#x200B;以完成程式。

在最終驗證之前，設定看起來應該類似以下範例：

![設定驗證](/help/overview/assets/log-forwarding/akamai/akamai-validation.png)
