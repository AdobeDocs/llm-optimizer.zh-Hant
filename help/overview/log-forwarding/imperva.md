---
title: 記錄轉送 — Imperva
description: 瞭解如何將CDN記錄檔從Imperva轉送至Adobe的S3儲存貯體，以便在LLM Optimizer中收集代理流量資料。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 3%

---


# 記錄轉送：Imperva {#log-forwarding-imperva}

本指南說明如何將CDN記錄檔從Imperva轉送至Adobe的S3儲存貯體，以進行代理流量資料收集。 您將使用LLM Optimizer CDN設定頁面加入LLM Optimizer。 上線流程完成後，請依照本頁面提供的步驟，從Imperva Web主控台設定記錄轉送。

## 步驟1：在LLM Optimizer中加入 {#step-1}

在LLM Optimizer頁面[https://llmo.now/](https://llmo.now/)上：

1. 移至&#x200B;**組態**。

   ![設定按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**CDN組態**」標籤。

   ![CDN設定索引標籤](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)
1. 按一下&#x200B;**開始使用**。
1. 在&#x200B;**啟用AI流量深入分析**&#x200B;旁邊，按一下&#x200B;**設定**。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)
1. 選取&#x200B;**Imperva (BYOCDN)**。

   ![選取Imperva](/help/overview/assets/log-forwarding/imperva/imperva-select.png)
1. 按一下&#x200B;**內建**。

## 步驟2：在Imperva中設定記錄轉送 {#step-2}

在[Imperva主控台](https://my.imperva.com)上：

>[!NOTE]
>
>記錄檔應每日傳送。

1. 在[https://my.imperva.com](https://my.imperva.com)登入您的Imperva帳戶。

2. 在側邊欄中，移至&#x200B;**記錄檔** > **記錄檔設定** （或&#x200B;**記錄檔整合**）。

3. 選取&#x200B;**Amazon S3 ARN**&#x200B;作為連線型別（記錄檔目的地）。

4. 輸入下列內容：

   | 欄位 | 說明 | 注意 |
   |---|---|---|
   | **連線名稱** | 連線的描述性名稱（例如生產S3記錄）。 您可以重新命名預設值。 | |
   | **路徑** | 將儲存記錄檔的資料夾位置。 使用格式`<Amazon S3 bucket name>/<log folder>`。 例如：`MyBucket/MyImpervaLogFolder`。 | `Amazon S3 bucket name`是來自LLM Optimizer設定頁面的&#x200B;**貯體名稱**。![儲存貯體名稱](/help/overview/assets/log-forwarding/imperva/imperva-bucket-name.png)記錄檔資料夾是來自LLM Optimizer設定頁面的&#x200B;**路徑**。![路徑](/help/overview/assets/log-forwarding/imperva/imperva-path.png) |

5. 按一下&#x200B;**測試連線**。 Imperva會執行完整測試，其中測試檔案（沒有真實資料）會傳送到指定的資料夾，然後在傳輸完成時移除。

   - **可用** — 儲存體詳細資料有效；您可以設定記錄檔以使用此連線。
   - **未定義** — 缺少必要的詳細資料或測試失敗。

6. 按一下&#x200B;**儲存**&#x200B;以儲存組態。

7. 設定記錄檔選項（記錄檔型別、記錄檔等級、格式、壓縮）和&#x200B;**記錄檔等級**。 您可以從LLM Optimizer設定頁面取得值。

   | 欄位 | 注意 |
   |---|---|
   | 記錄整合模式 | ![記錄整合模式](/help/overview/assets/log-forwarding/imperva/imperva-log-integration-mode.png) |
   | 傳遞方法 | ![傳遞方法](/help/overview/assets/log-forwarding/imperva/imperva-delivery-method.png) |
   | 記錄型別 | ![記錄型別](/help/overview/assets/log-forwarding/imperva/imperva-log-types.png) |
   | 記錄層級 | ![記錄層級](/help/overview/assets/log-forwarding/imperva/imperva-log-level.png) |
   | 格式 | ![格式](/help/overview/assets/log-forwarding/imperva/imperva-format.png) |
   | 壓縮記錄 | ![壓縮記錄檔](/help/overview/assets/log-forwarding/imperva/imperva-compress-logs.png) |
