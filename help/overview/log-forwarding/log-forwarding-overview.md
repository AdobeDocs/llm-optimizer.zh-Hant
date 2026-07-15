---
title: BYOCDN 記錄轉送概觀
description: 了解如何將內容傳遞網路記錄從您的服務提供者轉送至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
autotag-review: '2026-07-15T18:07:52.453Z'
TQID: 'https://experienceleague.adobe.com/iN1Tm-7j2FTQ1UodWvCZpOSy0FnQEyMkBMZIL9z3t38'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3aid: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2: id: d23587d6-14d6-4e3f-9ee1-cc18623832e1id: dd952468-5202-43af-a365-6e0d2e67a703id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: d3cdead0-685a-4489-9250-4bb709942f66id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 215
ht-degree: 100%

---


# BYOCDN 記錄轉送概觀 {#cdn-log-forwarding}

客戶管理之內容傳遞網路 (BYOCDN) 的記錄轉送是將您的內容傳遞網路存取記錄傳送至 Adobe 的 Amazon S3 貯體的流程，以便 LLM Optimizer 可以收集和分析代理式流量資料。 如果沒有內容傳遞網路記錄轉送，[代理式流量](/help/dashboards/agentic-traffic.md)儀表板就無法顯示量度。

以下提供的指南，遵循相同的兩階段工作流程：

1. **在 LLM Optimizer 上線**：在[內容傳遞網路設定](/help/dashboards/customer-configuration.md)頁面上註冊您的內容傳遞網路，以產生所需的 S3 認證和路徑詳細資料。
2. **設定您的內容傳遞網路**：使用這些詳細資料，在您的內容傳遞網路提供者的控制台中建立記錄轉送工作 (或手動上傳記錄)。 若為 CloudFront，您只能透過 **AWS CLI** 使用主控台或完成傳遞設定；請參閱 [CloudFront (AWS CLI)](/help/overview/log-forwarding/cloudfront-cli.md)。

## 內容傳遞網路提供者 {#cdn-providers}

請遵循您的內容傳遞網路提供者的相應指南。

| 內容傳遞網路提供者 | 指南 |
|---|---|
| Akamai | [查看指南](/help/overview/log-forwarding/akamai.md) |
| Cloudflare | [查看指南](/help/overview/log-forwarding/cloudflare.md) |
| CloudFront (主控台) | [查看指南](/help/overview/log-forwarding/cloudfront.md) |
| CloudFront (AWS CLI) | [查看指南](/help/overview/log-forwarding/cloudfront-cli.md) |
| Fastly | [查看指南](/help/overview/log-forwarding/fastly.md) |
| Imperva | [查看指南](/help/overview/log-forwarding/imperva.md) |
| 其他 (手動/不支援的內容傳遞網路) | [查看指南](/help/overview/log-forwarding/other.md) |

>[!NOTE]
>
>如果您的內容傳遞網路提供者未列於上方，請使用&#x200B;**其他 (手動/不支援的內容傳遞網路)** 指南，其中涵蓋手動上傳、臨時指令碼和任何非原生支援的內容傳遞網路。
