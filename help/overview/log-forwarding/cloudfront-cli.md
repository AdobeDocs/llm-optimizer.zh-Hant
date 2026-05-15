---
title: 記錄轉寄 - CloudFront (AWS CLI)
description: 使用 AWS CLI 將 CloudFront 內容傳遞網路記錄轉寄至 Adobe 的 S3 儲存貯體，進行傳遞設定和操作。
feature: Agentic Traffic
autotag-review: '2026-05-15T17:42:44.992Z'
TQID: 'https://experienceleague.adobe.com/NoVv3qv1RbtqAWGMPYC1Rz4wO-5Au1yL2e8tRKd9Hao'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
  - id: e69d5a42-0217-4ca5-9396-a9a826a170da
topic_v2:
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 379
ht-degree: 100%

---


# 記錄轉寄：CloudFront (AWS CLI) {#log-forwarding-cloudfront-cli}

本頁面詳細說明如何將內容傳遞網路記錄從 CloudFront 轉寄至 Adobe 的 S3 貯體，藉以彙集代理式流量資料。 您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。 上線流程完成後，請按照此頁面提供的步驟，使用[步驟 2](#step-2-cli) 的 [AWS 命令列介面](https://aws.amazon.com/cli/)設定記錄轉寄。

>[!NOTE]
>
> 本指南將說明如何使用 [AWS 命令列介面](https://aws.amazon.com/cli/)設定記錄轉寄。 如果您想要使用 **CloudFront 使用者介面**&#x200B;設定記錄轉寄，請參閱[記錄轉寄：CloudFront](/help/overview/log-forwarding/cloudfront.md)。

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

<!--  ![AWS Account ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)-->

1. 選取「**CloudFront (BYOCDN)**」。

   ![選取「CloudFront」](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. 按一下「**上線**」。

<!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步驟 2：使用 AWS CLI 設定內容傳遞網路記錄轉寄 {#step-2-cli}

使用 AWS CLI 設定內容傳遞網路記錄轉寄，如下所示：

### 設定 AWS CLI 認證

設定 AWS CLI 認證 MAC。 開啟 ~/.aws/credentials 並輸入以下變數的值：

```text
[LLMO]
aws_access_key_id=<VALUE_OF_ACCESS_KEY_ID>
aws_secret_access_key=<VALUE_OF_SECRET_KEY>
aws_session_token=<ONLY_IF_USING_SECURITY_TOKEN_SERVICE> ## Optional
```

### 測試連線

執行以下命令測試連線：

```bash
aws sts get-caller-identity --profile LLMO
```

成功輸出的範例：

```bash
aws sts get-caller-identity --profile LLMO
{
    "UserId": "AxxxxxxxxxxxP:user",
    "Account": "012345678912",
    "Arn": "arn:aws:sts::012345678912:assumed-role/klam-master-role-BatlY3dnPVinQLC/user"
}
```

### 初始化變數

將 `REPLACEME123@AdobeOrg` 取代為您的組織 Adobe IMS 組織 ID，然後執行以下命令。 此命令的輸出 ID 將名為 `TRANSFORM_IMS_ID`。

```bash
echo "REPLACEME123@AdobeOrg" | sed 's/@AdobeOrg$//' | tr '[:upper:]' '[:lower:]'
```

依照下列指引輸入 `CUSTOMER`、`CDN_ID`、`ACCT1` 和 `TRANSFORM_IMS_ID` 的值，然後從您的終端機執行命令。

```bash
export PROFILE1=LLMO
export REGION1=us-east-1
export CUSTOMER=<CUSTOMER_NAME> ## No Space, user letters,numbers and dash
export CDN_ID=<YOUR_CLOUDFRONT_DISTRIBUTION_ID>
export ACCT1=<YOUR_AWS_ACCOUNT_NUMBER>
export DELIVERY_DEST_ARN=arn:aws:logs:us-east-1:640168421876:delivery-destination:cdn-logs-<TRANSFORM_IMS_ID>-ams  ## Replace TRANSFORM_IMS_ID with the output of the command above
```

<!--Use the **Delivery destination ARN** and org values from the LLM Optimizer CDN configuration page if they differ from the pattern above.-->

### 建立傳遞來源

於和執行步驟 3 時相同的終端機，執行以下命令：

```bash
aws logs put-delivery-source --name llmo-cf-${CUSTOMER}-${CDN_ID} \
  --profile $PROFILE1 --region $REGION1 \
  --resource-arn arn:aws:cloudfront::${ACCT1}:distribution/${CDN_ID} \
  --log-type ACCESS_LOGS
```

>[!IMPORTANT]
>
>如果呈現以下錯誤，請搜尋既有的傳遞來源：*呼叫 PutDeliverySource 操作時發生錯誤 (ConflictException)：此 ResourceId 已用於此帳戶的其他傳遞來源。*
>
>若要搜尋既有的傳遞來源，請執行此命令：
>
>```bash
>aws logs describe-delivery-sources --region us-east-1 \
>--query "deliverySources[?contains(resourceArns[0], '<CDN DistributionID>')]"
>```
>
>在下一個命令中，使用上述命令結果中的傳遞來源名稱。

### 建立傳遞設定

```bash
aws logs create-delivery \
  --profile "$PROFILE1" --region "$REGION1" \
  --delivery-source-name "llmo-cf-${CUSTOMER}-${CDN_ID}" \
  --delivery-destination-arn $DELIVERY_DEST_ARN \
  --s3-delivery-configuration '{"suffixPath":"/{yyyy}/{MM}/{dd}/{HH}"}' \
  --record-fields 'date' 'time' 'x-edge-location' 'cs-method' 'cs(Host)' 'cs-uri-stem' 'sc-status' 'cs(Referer)' 'cs(User-Agent)' 'time-to-first-byte' 'sc-content-type' 'x-host-header'
```

&lt;!--若文件或產品值有所變更，請將 `--record-fields` 和 `--s3-delivery-configuration` 對應至您 LLM Optimizer 內容傳遞網路設定頁面中顯示的欄位清單與路徑後綴。-->
