---
title: 記錄轉送 - Imperva
description: 了解如何將內容傳遞網路記錄從 Imperva 轉送至 Adobe 的 S3 貯體，以便在 LLM Optimizer 中收集代理式流量資料。
feature: Agentic Traffic
autotag-review: '2026-07-15T17:57:30.264Z'
TQID: 'https://experienceleague.adobe.com/l-DYz7pXzFDqZn1rnZWUOG9PpRqosq00LmGrlsOMqNk'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3aid: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2: id: d23587d6-14d6-4e3f-9ee1-cc18623832e1id: dd952468-5202-43af-a365-6e0d2e67a703id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: d3cdead0-685a-4489-9250-4bb709942f66id: e1e0219c-f879-479f-8427-888ed2a6e9c2id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 352
ht-degree: 93%

---


# 記錄轉送：Imperva {#log-forwarding-imperva}

本指南說明如何將內容傳遞網路記錄從 Imperva 轉送至 Adobe 的 S3 貯體，以收集代理式流量資料。 您將使用 LLM Optimizer 內容傳遞網路設定頁面，在 LLM Optimizer 上線。 上線流程完成後，請依照本頁面提供的步驟，從 Imperva 網頁控制台設定記錄轉送。

## 步驟 1：在 LLM Optimizer 上線 {#step-1}

在 LLM Optimizer 頁面 [https://llmo.now/](https://llmo.now/)：

1. 前往「**設定**」。

   ![「設定」按鈕](/help/overview/assets/log-forwarding/common/config-button.png)

1. 按一下「**內容傳遞網路設定**」分頁。

   ![「內容傳遞網路設定」分頁](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)
1. 按一下「**開始使用**」。
1. 在「**啟用 AI 流量洞察**」旁邊，按一下「**設定**」。

   ![設定](/help/overview/assets/log-forwarding/common/configure.png)
1. 選取「**Imperva (BYOCDN)**」。

   ![選取「Imperva」](/help/overview/assets/log-forwarding/imperva/imperva-select.png)
1. 按一下「**上線**」。

## 步驟 2：在 Imperva 中設定記錄轉送 {#step-2}

在 [Imperva 控制台](https://my.imperva.com)上：

>[!NOTE]
>
>記錄應每日傳送。

1. 在 [https://my.imperva.com](https://my.imperva.com) 登入您的 Imperva 帳戶。

2. 在側邊欄中，前往「**記錄**」>「**記錄設定**」(或「**記錄整合**」)。

3. 選取「**Amazon S3 ARN**」作為連線類型 (記錄目標)。

4. 輸入下列內容：

   | 欄位 | 說明 | 注意 |
   |---|---|---|
   | **連線名稱** | 連線的說明性名稱 (例如生產 S3 記錄)。 您可將預設名稱重新命名。 | |
   | **路徑** | 將要儲存記錄檔案的資料夾之位置。 使用 `<Amazon S3 bucket name>/<log folder>` 格式。 例如：`MyBucket/MyImpervaLogFolder`。 | `Amazon S3 bucket name`是來自LLM Optimizer設定頁面的&#x200B;**貯體名稱**。 ![儲存貯體名稱](/help/overview/assets/log-forwarding/imperva/imperva-bucket-name.png)記錄檔資料夾是來自LLM Optimizer設定頁面的&#x200B;**路徑**。 ![路徑](/help/overview/assets/log-forwarding/imperva/imperva-path.png) |

5. 按一下「**測試連線**」。 Imperva 會執行完整測試，過程中會將一個測試檔案 (不含真實資料) 傳送到指定的資料夾，然後在轉移完成時移除。

   - **可用**：儲存空間詳細資料有效；您可以設定記錄以使用此連線。
   - **未定義**：缺少必要的詳細資料或測試失敗。

6. 按一下「**儲存**」以儲存設定。

7. 設定記錄選項 (記錄類型、記錄層級、格式、壓縮) 和「**記錄層級**」。 您可以從 LLM Optimizer 設定頁面取得值。

   | 欄位 | 注意 |
   |---|---|
   | 記錄整合模式 | ![記錄整合模式](/help/overview/assets/log-forwarding/imperva/imperva-log-integration-mode.png) |
   | 傳遞方法 | ![傳遞方法](/help/overview/assets/log-forwarding/imperva/imperva-delivery-method.png) |
   | 記錄類型 | ![記錄類型](/help/overview/assets/log-forwarding/imperva/imperva-log-types.png) |
   | 記錄層級 | ![記錄層級](/help/overview/assets/log-forwarding/imperva/imperva-log-level.png) |
   | 格式 | ![格式](/help/overview/assets/log-forwarding/imperva/imperva-format.png) |
   | 壓縮記錄 | ![壓縮記錄](/help/overview/assets/log-forwarding/imperva/imperva-compress-logs.png) |
