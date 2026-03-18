---
title: BYOCDN記錄轉送概觀
description: 瞭解如何從您的提供者將CDN記錄檔轉送至Adobe的S3儲存貯體，以便在LLM Optimizer中收集代理流量資料。
feature: Agentic Traffic
source-git-commit: a1ba7684ccef9baf3452cc158fc0d6a12aa7adb8
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 3%

---


# BYOCDN記錄轉送概觀 {#cdn-log-forwarding}

客戶管理的CDN (BYOCDN)的記錄轉送是將您的CDN存取記錄傳送至Adobe的Amazon S3儲存貯體的程式，以便LLM Optimizer可以收集和分析代理流量資料。 如果沒有CDN記錄轉送，[代理流量](/help/dashboards/agentic-traffic.md)儀表板就無法顯示量度。

以下提供指南，遵循相同的兩階段工作流程：

1. **加入LLM Optimizer** — 在[CDN設定](/help/dashboards/customer-configuration.md)頁面上註冊您的CDN，以產生所需的S3憑證和路徑詳細資料。
2. **設定您的CDN** — 使用這些詳細資料在您的CDN提供者的主控台中建立記錄轉送工作（或手動上傳記錄檔）。

## CDN提供者 {#cdn-providers}

請遵循您CDN提供者對應的指南。

| 內容傳遞網路提供者 | 指南 |
|---|---|
| Akamai | [檢視指南](/help/overview/log-forwarding/akamai.md) |
| Cloudflare | [檢視指南](/help/overview/log-forwarding/cloudflare.md) |
| CloudFront | [檢視指南](/help/overview/log-forwarding/cloudfront.md) |
| Fastly | [檢視指南](/help/overview/log-forwarding/fastly.md) |
| imperva | [檢視指南](/help/overview/log-forwarding/imperva.md) |
| 其他（手動/不支援的CDN） | [檢視指南](/help/overview/log-forwarding/other.md) |

>[!NOTE]
>
>如果您的CDN提供者未列於上面，請使用&#x200B;**其他（手動/不支援的CDN）**&#x200B;指南，該指南涵蓋手動上傳、臨機指令碼和任何原生不支援的CDN。
