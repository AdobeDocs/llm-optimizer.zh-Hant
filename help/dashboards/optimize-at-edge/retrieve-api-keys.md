---
title: 擷取您的API金鑰
description: 如何從LLM Optimizer擷取生產及測試Edge最佳化API金鑰。
feature: Opportunities
source-git-commit: 3b6dc163f4488a22937916beb6778de4abc5a20c
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 18%

---


# 擷取您的API金鑰

在設定CDN之前，請先從LLM Optimizer UI擷取Edge最佳化API金鑰。 您需要&#x200B;**生產** API金鑰才能使用即時流量。 您也可選擇擷取&#x200B;**staging** API金鑰，以先測試中繼主機名稱上的路由。

## 生產API金鑰

1. 在 LLM Optimizer 中，開啟「**客戶設定**」並選取「**內容傳遞網路設定**」標籤。

   ![導覽至客戶設定](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找出「**將最佳化部署到 AI 代理**」區段。 勾選「**啟用最佳化引擎**」核取方塊。

   ![將最佳化部署到 AI 代理：待處理](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在確認對話框中，選取「**啟用**」。

   ![啟用最佳化引擎確認對話框](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. 選取&#x200B;**檢視詳細資料**。 在&#x200B;**部署最佳化詳細資料**&#x200B;對話方塊中，複製&#x200B;**生產API金鑰** （使用欄位旁的&#x200B;**複製**）。

   部署最佳化詳細資料中的![生產API金鑰](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >對話方塊可能會顯示設定未完成。 在驗證路由之前，這是預期中的情形 — 您仍可複製API金鑰，讓您的IT或CDN團隊完成設定。

如果您需要上述步驟的任何協助，請聯絡您的Adobe帳戶團隊或`llmo-at-edge@adobe.com`。

## 測試API金鑰（選擇性）

若要在啟用生產製程之前，先在較低的環境中驗證製程，您可以設定暫存主機名稱。

**需求**

* 暫存主機名稱必須位於與生產環境相同的&#x200B;**可登入網域**&#x200B;上（例如，當生產環境為`https://www.example.com`時，`https://staging.example.com`）。
* 每個網站只有&#x200B;**一個**&#x200B;暫存網域。 儲存後，必須聯絡Adobe才能變更。

**步驟**

1. 在 LLM Optimizer 中，開啟「**客戶設定**」並選取「**內容傳遞網路設定**」標籤。
2. 在&#x200B;**將最佳化部署到AI代理程式**&#x200B;下，選取&#x200B;**新增中繼網域** （或如果已設定中繼網域，則選取&#x200B;**中繼網域**）。
3. 輸入包含`https://`的完整暫存URL並選取&#x200B;**設定網域**。
4. 從確認對話方塊複製&#x200B;**staging** API金鑰。

   ![中繼網域API金鑰](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

使用中繼API金鑰在中繼環境中部署相同的路由規則。

如果您需要協助，請連絡`llmo-at-edge@adobe.com`。

## 後續步驟

擷取API金鑰後，請返回[CDN安裝指南](/help/dashboards/optimize-at-edge/overview.md#cdn-configuration-guides)以設定路由。
