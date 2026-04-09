---
title: 記錄轉送 — CloudFront (AWS CLI)
description: 使用AWS CLI將CloudFront CDN記錄轉送至Adobe的S3儲存貯體，以進行傳遞設定和操作。
feature: Agentic Traffic
source-git-commit: 0d51bbde954c399dc6595522fa70b576461f458a
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 20%

---


# 記錄轉送： CloudFront (AWS CLI) {#log-forwarding-cloudfront-cli}

本頁詳細說明如何將CDN記錄從CloudFront轉送至Adobe的S3儲存貯體，以進行代理流量資料收集。 您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。 上線流程完成後，請依照本頁面提供的步驟，在[步驟2](#step-2-cli)中使用[AWS命令列介面](https://aws.amazon.com/cli/)設定記錄轉送。

>[!NOTE]
>
> 本指南說明如何使用[AWS命令列介面](https://aws.amazon.com/cli/)來設定記錄轉送。 如果您想要使用&#x200B;**CloudFront UI**&#x200B;設定記錄轉送，請參閱[記錄轉送： CloudFront](/help/overview/log-forwarding/cloudfront.md)。

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

## 步驟2：使用AWS CLI設定CDN記錄轉送 {#step-2-cli}

使用AWS CLI設定CDN記錄轉送，如下所示：

### 設定AWS CLI認證

設定AWS CLI認證MAC。 開啟~/.aws/credentials並輸入以下變數的值：

```text
[LLMO]
aws_access_key_id=<VALUE_OF_ACCESS_KEY_ID>
aws_secret_access_key=<VALUE_OF_SECRET_KEY>
aws_session_token=<ONLY_IF_USING_SECURITY_TOKEN_SERVICE> ## Optional
```

### 測試連線

執行以下命令以測試連線：

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

將`REPLACEME123@AdobeOrg`取代為您的組織Adobe IMS組織ID，然後執行以下命令。 這個命令的輸出ID將參照為`TRANSFORM_IMS_ID`。

```bash
echo "REPLACEME123@AdobeOrg" | sed 's/@AdobeOrg$//' | tr '[:upper:]' '[:lower:]'
```

依照下列指引輸入`CUSTOMER`、`CDN_ID`、`ACCT1`和`TRANSFORM_IMS_ID`的值，然後從您的終端機執行命令。

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

從執行步驟3的同一終端機，執行以下命令：

```bash
aws logs put-delivery-source --name llmo-cf-${CUSTOMER}-${CDN_ID} \
  --profile $PROFILE1 --region $REGION1 \
  --resource-arn arn:aws:cloudfront::${ACCT1}:distribution/${CDN_ID} \
  --log-type ACCESS_LOGS
```

>[!IMPORTANT]
>
>如果您收到下列錯誤，請搜尋現有的傳遞來源： *呼叫PutDeliverySource作業時發生錯誤(ConflictException)：此ResourceId已用於此帳戶的其他傳遞Source。*
>
>若要搜尋現有的傳遞來源，請執行此命令：
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

&lt;！ — 如果檔案或產品值變更，將`--record-fields`和`--s3-delivery-configuration`與LLM Optimizer CDN設定頁面上顯示的欄位清單和路徑尾碼對齊。—>
