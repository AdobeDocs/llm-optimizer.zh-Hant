---
title: 記錄轉送 — CloudFront
description: 瞭解如何將CDN記錄檔從CloudFront轉送至Adobe的S3貯體，以便在LLM Optimizer中收集代理流量資料。
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 0%

---


# 記錄轉送： CloudFront {#log-forwarding-cloudfront}

本頁面說明如何將CDN記錄檔從CloudFront轉送至Adobe的S3儲存貯體，以進行代理流量資料收集。 您將使用LLM Optimizer CDN設定頁面加入LLM Optimizer。 完成上線流程後，請依照本頁面提供的步驟，在CloudFront儀表板主控台中設定記錄轉送。

## 步驟1：在LLM Optimizer中加入 {#step-1}

在LLM Optimizer頁面[https://llmo.now/](https://llmo.now/)上：

1. 前往&#x200B;**客戶設定儀表板**。

   ![設定按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**CDN組態**」標籤。

   ![CDN設定索引標籤](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下&#x200B;**開始使用**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**啟用AI流量深入分析**&#x200B;旁邊，按一下&#x200B;**設定**。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 輸入您的&#x200B;**AWS帳戶** ID。

   ![AWS帳戶ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)

1. 選取&#x200B;**CloudFront (BYOCDN)**。

   ![選取CloudFront](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. 按一下&#x200B;**內建**。

   ![Onboard按鈕](/help/overview/assets/log-forwarding/common/onboard-button.png)

## 步驟2：啟用標準記錄（CloudFront主控台） {#step-2}

若要啟用標準記錄，請從[AWS管理主控台](https://aws.amazon.com/console/)：

1. 存取[CloudFront主控台](https://console.aws.amazon.com/cloudfront/v4/home)並[更新現有的分發](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToUpdateDistribution.html#HowToUpdateDistributionProcedure)。

1. 開啟&#x200B;**記錄**&#x200B;標籤。

1. 選擇&#x200B;**新增**，然後選取要接收記錄檔的服務，在此案例中為&#x200B;**Amazon S3**。

1. 針對&#x200B;**目的地**，請選取或建立資源。 輸入&#x200B;**儲存貯體名稱**，您可以從LLM Optimizer CDN設定頁面複製值。

   ![CloudFront儲存貯體名稱](/help/overview/assets/log-forwarding/cloudfront/cloudfront-bucket-name.png)

1. 設定&#x200B;**其他設定**：

   - **欄位選擇** — 選擇記錄檔欄位。 請參閱LLM Optimizer CDN設定頁面上的必要欄位。

     ![CloudFront欄位選擇](/help/overview/assets/log-forwarding/cloudfront/cloudfront-field-selection.png)

   - **分割** — 從LLM Optimizer設定頁面複製&#x200B;**路徑尾碼**。

     ![CloudFront分割](/help/overview/assets/log-forwarding/cloudfront/cloudfront-partitioning.png)

   - **輸出格式** — 格式應為JSON。

     ![CloudFront輸出格式](/help/overview/assets/log-forwarding/cloudfront/cloudfront-output-format.png)

1. 完成更新或建立發佈的步驟。

1. 在&#x200B;**記錄檔**&#x200B;頁面上，確認&#x200B;**已啟用**&#x200B;出現在散發旁。

## 為跨帳戶傳遞啟用標準記錄 {#cross-account}

**來源帳戶** （具有CloudFront分配）傳送存取記錄至&#x200B;**目的地帳戶** （LLM Optimizer CDN設定頁面中顯示的S3貯體）。 兩個帳戶都必須有正確的許可權。

例如：來源帳戶`111111111111`會將記錄檔傳送到目的地帳戶`222222222222`中的S3貯體。 您可以使用[AWS Commad Line介面](https://aws.amazon.com/cli/)。

>[!NOTE]
>
>在下列命令中，從LLM Optimizer設定頁面將傳遞目的地ARN值(`arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination`)取代為&#x200B;**傳遞目的地ARN**&#x200B;的值。

![傳遞目的地ARN](/help/overview/assets/log-forwarding/cloudfront/cloudfront-delivery-destination-arn.png)

### 設定來源帳戶 {#source-account}

接下來，您需要設定來源帳戶：

1. **建立傳遞來源** — 取代名稱和散發ARN：

   ```bash
   aws logs put-delivery-source --name s3-cf-delivery \
     --resource-arn arn:aws:cloudfront::111111111111:distribution/E1TR1RHV123ABC \
     --log-type ACCESS_LOGS
   ```

1. **建立傳遞** — 將來源連結至目的地；從[設定目的地帳戶]步驟使用目的地ARN：

   ```bash
   aws logs create-delivery --delivery-source-name s3-cf-delivery \
     --delivery-destination-arn arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination
   ```

1. **驗證：**

   - 在&#x200B;**來源**&#x200B;帳戶中： CloudFront主控台>您的發佈> **記錄**&#x200B;索引標籤。 在&#x200B;**型別**&#x200B;底下，您應該會看到S3跨帳戶記錄傳遞。
   - 在&#x200B;**目的地**&#x200B;帳戶中： S3主控台>貯體。 您應該會看到前置詞（例如`MyLogPrefix`）以及該資料夾中的記錄檔。
