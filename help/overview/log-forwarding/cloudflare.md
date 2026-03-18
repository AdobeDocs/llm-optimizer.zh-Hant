---
title: 記錄轉送 — Cloudflare
description: 瞭解如何將CDN記錄檔從Cloudflare轉送至Adobe的S3貯體，以便在LLM Optimizer中收集代理流量資料。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# 記錄轉送： Cloudflare {#log-forwarding-cloudflare}

本頁面詳細說明如何將CDN記錄檔從Cloudflare轉送至Adobe的S3儲存貯體，以進行代理流量資料收集。 您將使用LLM Optimizer CDN設定頁面加入LLM Optimizer。 上線流程完成後，請依照本頁提供的步驟，在Cloudflare儀表板主控台中設定記錄轉送。

## 步驟1：在LLM Optimizer中加入 {#step-1}

在LLM Optimizer頁面[https://llmo.now/](https://llmo.now/)上：

1. 前往&#x200B;**客戶設定儀表板**。

   ![設定按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**CDN組態**」標籤。

   ![CDN設定索引標籤](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下&#x200B;**開始使用**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png) -->

1. 在&#x200B;**啟用AI流量深入分析**&#x200B;旁邊，按一下&#x200B;**設定**。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 選取&#x200B;**Cloudflare (BYOCDN)**。

   ![選取Cloudflare](/help/overview/assets/log-forwarding/cloudflare/cloudflare-select.png)

1. 按一下&#x200B;**內建**。

   <!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步驟2：在Cloudflare中建立Logpush工作 {#step-2}

在[Cloudflare儀表板](https://dash.cloudflare.com/login)上，請遵循下列步驟：

1. 移至&#x200B;**網域（區域）**&#x200B;層級的&#x200B;**Logpush**&#x200B;頁面。
1. 選取&#x200B;**建立Logpush工作**。
1. 在&#x200B;**選取目的地**&#x200B;中，選擇&#x200B;**Amazon S3**。
1. 輸入下列目的地資訊：

   - **Bucket** — S3 Bucket名稱。 從LLM Optimizer CDN設定頁面複製值。

     ![儲存貯體名稱](/help/overview/assets/log-forwarding/common/bucket-name.png)

   - **路徑** — 儲存容器中的貯體位置。 從LLM Optimizer CDN設定頁面複製值。

     ![Cloudflare路徑](/help/overview/assets/log-forwarding/cloudflare/cloudflare-path.png)

   - **將記錄檔整理到每日子資料夾** （建議使用）。

     ![每日子資料夾](/help/overview/assets/log-forwarding/cloudflare/cloudflare-daily-subfolders.png)

   - **貯體區域** — 從LLM Optimizer CDN設定頁面複製值。

     <!-- ![Region](/help/overview/assets/log-forwarding/cloudflare/cloudflare-region.png)-->

   - 如果不需要伺服器端加密，請保持未勾選狀態。

   完成上述步驟後，請選取&#x200B;**繼續**。

1. 為了證明所有權，Cloudflare會將檔案傳送至您指定的目的地。 若要尋找Token，請按一下所有權挑戰檔案&#x200B;**總覽**&#x200B;索引標籤中的&#x200B;**開啟**&#x200B;按鈕。 從LLM Optimizer CDN設定頁面複製擁有權權權杖，然後貼到Cloudflare儀表板中，驗證您對貯體的存取權。 輸入所有權權杖並選取&#x200B;**繼續**。

   <!--![Ownership token](/help/overview/assets/log-forwarding/cloudflare/cloudflare-ownership-token.png)-->

1. 選取&#x200B;**HTTP要求**&#x200B;資料集，以推送至儲存服務。

1. 設定您的Logpush工作：

   - 輸入&#x200B;**工作名稱**。

   - 在&#x200B;**傳送下列欄位**&#x200B;中，檢視LLM Optimizer設定頁面中的值。

     ![Logpush欄位](/help/overview/assets/log-forwarding/cloudflare/cloudflare-logpush-fields.png)

   - **記錄格式**： JSON。

     <!--![JSON format](/help/overview/assets/log-forwarding/cloudflare/cloudflare-json-format.png)-->

1. 在&#x200B;**進階選項**&#x200B;中：

   - 選擇記錄檔中時間戳記欄位的格式： `RFC3339`。

     ![時間戳記格式](/help/overview/assets/log-forwarding/cloudflare/cloudflare-timestamp-format.png)

   - 針對取樣速率，選取&#x200B;**所有記錄**。

     ![取樣速率](/help/overview/assets/log-forwarding/cloudflare/cloudflare-sampling-rate.png)

1. 完成設定Logpush工作之後，請選取&#x200B;**提交**。
