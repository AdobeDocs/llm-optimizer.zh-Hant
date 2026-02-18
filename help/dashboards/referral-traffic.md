---
title: 轉介流量
description: 了解如何使用轉介流量儀表板來查看訪客如何從外部平台、AI 引用和轉介連結抵達您的網站。
feature: Referral Traffic
source-git-commit: e50c87e8e5a669526f3c10855c1629ce82b67aef
workflow-type: ht
source-wordcount: '605'
ht-degree: 100%

---


# 轉介流量

轉介流量會顯示訪客如何從外部平台、AI 引用和轉介連結抵達您的網站。轉介流量會追蹤和分析來自外部網站及平台的流量來源、轉介模式和轉換量度。您可以藉此了解哪些來源、區域和頁面帶來參與度最高的流量。<!--Data is sourced from the CDN logs, a privacy-preserving source that does not capture personal user data.-->您也可以利用自訂篩選器來精確調整所顯示的資料。

![轉介頁面](/help/dashboards/assets/referral-traffic.png)

此頁面詳細說明下列內容：

* [設定](#setup)
* [篩選器](#filters)
* [整體轉介效能](#overall-performance)
* [熱門轉介 URL](#top-referrals)
* [轉介流量詳細資料](#traffic-details)

## 設定 {#setup}

第一次登入時，轉介流量儀表板可能會顯示空白。您必須設定「[內容傳遞網路記錄轉送](/help/dashboards/customer-configuration.md#cdn-configuration)」才能檢視您的資料，請選取「**移至設定**」進行設定。

![轉介設定](/help/dashboards/assets/referral-setup1.png)

<!--- 1. Select your Source (either CDN logs or AEM Operational Telemetry).
2. Enter a primary contact email.
3. Click **Request activation** to enable data ingestion. Hiding this until confirmation from PM-->

在啟動後，儀表板將會填入轉介流量量度。

## 篩選器 {#filters}

在頁面頂端，您可以套用篩選器來精確調整視圖。您選擇的篩選器將影響儀表板上&#x200B;**所有**&#x200B;區段。您可以自訂下列項目：

* **日期範圍**：選取所顯示資料的時間範圍。例如最近 4 週。您也可選取「**自訂週數**」選項來自訂時段。
* **平台**：選擇特定流量來源，例如 Google、OpenAI 或社交媒體。
* **頁面意圖**：依使用者意圖篩選轉介流量。
* **管道來源**：依管道的來源進行篩選。其中選項包括 LLM、外部賺取、付費或混合轉介管道。
* **裝置類型**：依訪客的裝置類型 (桌上型電腦、行動裝置或所有裝置) 分析流量。
  **區域**：檢視不同地理區域的轉介模式。

選取想要的篩選器後，按一下「**套用篩選器**」，將選取項目套用至儀表板。

## 整體轉介效能 {#overall-performance}

儀表板會顯示多項主要量度，以便凸顯整體轉介效能，而這些量度包括：

* **總轉介流量**：來自所有來源的總轉介流量。
* **來自 LLM 的轉介流量**：來自 LLM 的總轉介流量。
* **同意率**：訪客接受同意提示的百分比。
* **跳出率**：來自轉介來源但是沒有任何參與事件的工作階段之百分比。

![轉介頁面](/help/dashboards/assets/referral-traffic.png)

除了上述整體效能量度外，另有三個面板顯示橫跨不同市場、轉介來源和頁面意圖類別的流量分佈<!-- the **Top Regions** panel breaks down traffic by geography. Meanwhile, the **Top Referral Sources** panel shows the platforms driving the most visits. Trend indicators for the metrics show how these values are changing over time compared to the previous period.-->

<!--## Top Referral URLs {#top-referrals}

The Top Referral URLs list surfaces your site's most visited pages from referrals.

![Top Referral URLs](/help/dashboards/assets/top-url.png)-->

## 轉介來源詳細資料和 URL 效能分析 {#traffic-details}

「轉介來源詳細資料」和「URL 效能分析」表格可協助您評估流量的數量和品質。點按下列每個分頁標籤以取得詳細資料：

![轉介流量詳細資料](/help/dashboards/assets/traffic-details.png)

>[!BEGINTABS]

>[!TAB 轉介來源詳細資料]

「轉介來源詳細資料」視圖將來自不同平台 (例如 OpenAI、Microsoft、Google 和 Perplexity) 的流量進行劃分。此視圖呈現多個主要量度，例如造訪次數、跳出率和管道類型，協助您了解哪些 AI 和搜尋來源為您的網站帶來最多參與流量。

* **來源**：轉介流量的來源。
* **造訪次數**：每個來源的造訪總次數。
* **跳出率**：來自轉介來源但是沒有任何參與事件的工作階段之百分比。
* **管道**：來源的管道，包括贏取、付費或兩者。

>[!TAB URL 效能分析]

「URL 效能分析」視圖會根據來自 LLM 和其他來源的轉介流量數量，針對表現最佳的頁面進行排名。此視圖會特別標示例如流量、跳出率、同意率和頁面意圖等多項量度，協助您識別出哪些頁面能夠吸引並留住透過 AI 轉介前來的參與度最高的訪客。該表格具有方便快速存取各個主題的搜尋欄位

>[!ENDTABS]

您在這兩個表格上皆可使用「**匯出**」選項來下載 .csv 格式的表格，並與您的團隊共用洞察或將表格納入執行報告中。此外，您在這兩個表格上皆可按一下「**設定欄**」按鈕來自訂要顯示的量度。
