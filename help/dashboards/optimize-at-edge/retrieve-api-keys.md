---
title: 檢索 API 金鑰
description: 如何從 LLM Optimizer 檢索生產及中繼 Edge Optimize API 金鑰。
feature: Opportunities
autotag-review: '2026-07-15T18:05:12.505Z'
TQID: 'https://experienceleague.adobe.com/X3vIzxrlaqJ5Mx3K8rOpX2QqgGyxT6p8qfdLzTWC1gQ'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 337
ht-degree: 100%

---


# 檢索 API 金鑰

在設定內容傳遞網路之前，請先從 LLM Optimizer UI 檢索 Edge Optimize API 金鑰。 處理即時流量必須使用&#x200B;**生產** API 金鑰。 或者，您也可以先檢索&#x200B;**中繼** API 金鑰以測試中繼主機名稱上的路由。

## 生產 API 金鑰

1. 在 LLM Optimizer 中，開啟「**客戶設定**」並選取「**內容傳遞網路設定**」標籤。

   ![導覽至客戶設定](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找出「**將最佳化部署到 AI 代理**」區段。 勾選「**啟用最佳化引擎**」核取方塊。

   ![將最佳化部署到 AI 代理：待處理](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在確認對話框中，選取「**啟用**」。

   ![啟用最佳化引擎確認對話框](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. 選取&#x200B;**「檢視詳細資訊」**。 在&#x200B;**「部署最佳化詳細資訊」**&#x200B;對話框中，複製&#x200B;**「生產 API 金鑰」** (使用欄位旁的&#x200B;**「複製」**)。

   ![部署最佳化詳細資訊中的生產 API 金鑰](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >對話框可能會顯示設定未完成。 在路由驗證完成之前，這屬於正常情況。您仍可複製 API 金鑰，讓您的 IT 或內容傳遞網路團隊完成設定。

如果您需要協助完成上述步驟，請聯絡您的 Adobe 帳戶團隊或 `llmo-at-edge@adobe.com`。

## 中繼 API 金鑰 (選用)

若要在啟用生產路由之前先在非正式環境中驗證路由，您可以設定中繼主機名稱。

**需求**

* 中繼主機名稱必須與生產環境位於相同的&#x200B;**可註冊網域**&#x200B;上 (例如，若生產環境是 `https://www.example.com`，中繼主機名稱便是 `https://staging.example.com`)。
* 每個網站只有&#x200B;**一個**&#x200B;中繼網域。 儲存後，必須聯絡 Adobe 才能變更。

**步驟**

1. 在 LLM Optimizer 中，開啟「**客戶設定**」並選取「**內容傳遞網路設定**」標籤。
2. 在&#x200B;**「將最佳化部署到 AI 代理」**&#x200B;下，選取&#x200B;**「新增中繼網域」** (或如果已設定中繼網域，則選取&#x200B;**「中繼網域」**)。
3. 輸入包含 `https://` 的完整中繼 URL 並選取&#x200B;**「設定網域」**。
4. 複製確認對話框中的&#x200B;**中繼** API 金鑰。

   ![中繼網域 API 金鑰](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

使用中繼 API 金鑰在中繼環境中部署相同的路由規則。

如果您需要協助，請聯絡 `llmo-at-edge@adobe.com`。

## 後續步驟

在擷取 API 金鑰後，請返回[內容傳遞網路設定指南](/help/dashboards/optimize-at-edge/overview.md#cdn-configuration-guides)以設定路由。
