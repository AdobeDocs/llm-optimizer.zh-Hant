---
title: 記錄轉送 - CloudFront
description: 了解如何將內容傳遞網路記錄從 CloudFront 轉送至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
autotag-review: '2026-07-15T17:47:22.372Z'
TQID: 'https://experienceleague.adobe.com/0aPeInYmcNRZLHUdABG2cEpT-dXb6GhEMoNUqMLMusY'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
  - id: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
  - id: e69d5a42-0217-4ca5-9396-a9a826a170da
  - id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 466
ht-degree: 100%

---


# 記錄轉送：CloudFront {#log-forwarding-cloudfront}

本頁面說明如何將內容傳遞網路記錄從 CloudFront 轉送至 Adobe 的 S3 貯體，以收集代理式流量資料。 您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。 完成上線流程後，請依照本頁面提供的步驟，在 CloudFront 儀表板控制台中設定記錄轉送。

## 步驟 1：在 LLM Optimizer 上線 {#step-1}

在 LLM Optimizer 頁面 [https://llmo.now/](https://llmo.now/)：

1. 前往「**客戶設定儀表板**」。

   ![「設定」按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**內容傳遞網路設定**」分頁。

   ![「內容傳遞網路設定」分頁](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 按一下「**開始使用**」。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在「**啟用 AI 流量洞察**」旁邊，按一下「**設定**」。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)

1. 輸入您的 **AWS 帳戶** ID。

   ![AWS 帳戶 ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)

1. 選取「**CloudFront (BYOCDN)**」。

   ![選取「CloudFront」](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. 按一下「**上線**」。

   ![「上線」按鈕](/help/overview/assets/log-forwarding/common/onboard-button.png)

## 步驟 2：啟用標準記錄 (CloudFront 控制台) {#step-2}

若要啟用標準記錄，請從 [AWS 管理控制台](https://aws.amazon.com/console/)：

1. 存取 [CloudFront 控制台](https://console.aws.amazon.com/cloudfront/v4/home)，並[更新現有的分發](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToUpdateDistribution.html#HowToUpdateDistributionProcedure)。

1. 開啟「**記錄**」分頁。

1. 選擇「**新增**」，然後選取要接收記錄的服務，在此案例中為 **Amazon S3**。

1. 針對「**目標**」，請選取或建立資源。 輸入&#x200B;**貯體名稱**，您可以從 LLM Optimizer 內容傳遞網路設定頁面複製其值。

   ![CloudFront 貯體名稱](/help/overview/assets/log-forwarding/cloudfront/cloudfront-bucket-name.png)

1. 設定&#x200B;**其他設定**：

   - **選取欄位**：選擇記錄檔案欄位。 請參閱 LLM Optimizer 內容傳遞網路設定頁面上的必要欄位。

     ![CloudFront 欄位選取](/help/overview/assets/log-forwarding/cloudfront/cloudfront-field-selection.png)

   - **分割**：複製 LLM Optimizer 設定頁面中的&#x200B;**路徑後綴**。

     ![CloudFront 分割](/help/overview/assets/log-forwarding/cloudfront/cloudfront-partitioning.png)

   - **輸出格式**：格式應為 JSON。

     ![CloudFront 輸出格式](/help/overview/assets/log-forwarding/cloudfront/cloudfront-output-format.png)

1. 完成更新或建立分發的步驟。

1. 在「**記錄**」頁面上，確認在分發旁邊顯示「**已啟用**」。

## 啟用跨帳戶傳遞的標準記錄 {#cross-account}

**來源帳戶** (具有 CloudFront 分發) 傳送存取記錄至&#x200B;**目標帳戶** (LLM Optimizer 內容傳遞網路設定頁面中顯示的 S3 貯體)。 兩個帳戶都必須有正確的權限。

例如：來源帳戶 `111111111111` 將記錄傳送到目標帳戶 `222222222222` 中的 S3 貯體。 您可以使用 [AWS 命令列介面](https://aws.amazon.com/cli/)。

>[!NOTE]
>
>在下列命令中，將傳遞目標 ARN 的值 (`arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination`) 替換為 LLM Optimizer 設定頁面中的「**傳遞目標 ARN** 」的值。

![傳遞目標 ARN](/help/overview/assets/log-forwarding/cloudfront/cloudfront-delivery-destination-arn.png)

### 設定來源帳戶 {#source-account}

接下來，您需要設定來源帳戶：

1. **建立傳遞來源**：替換名稱和分發 ARN：

   ```bash
   aws logs put-delivery-source --name s3-cf-delivery \
     --resource-arn arn:aws:cloudfront::111111111111:distribution/E1TR1RHV123ABC \
     --log-type ACCESS_LOGS
   ```

1. **建立傳遞** - 將來源連結至目標；使用「設定目標帳戶」步驟中的目標 ARN：

   ```bash
   aws logs create-delivery --delivery-source-name s3-cf-delivery \
     --delivery-destination-arn arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination
   ```

1. **驗證：**

   - 在&#x200B;**來源**&#x200B;帳戶中：CloudFront 控制台 > 您的分發 > 「**記錄**」分頁。 在「**類型**」下，您應該會看到 S3 跨帳戶記錄傳遞。
   - 在&#x200B;**目標**&#x200B;帳戶中：S3 控制台 > 貯體。 您應該會看到前置詞 (例如 `MyLogPrefix`) 以及該資料夾中的記錄。
