---
title: 反向連結流量
description: 瞭解如何使用反向連結流量儀表板，以檢視訪客如何從外部平台、AI引文和反向連結進入您的網站。
source-git-commit: a699f8f3c50f77d07f29cd354fd1ef8e6eed8ff9
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---


# 反向連結流量

反向連結流量會顯示訪客如何從外部平台、AI引文和反向連結進入您的網站。 它會追蹤及分析來自外部網站與平台的流量來源、反向連結模式與轉換量度。 這可協助您瞭解哪些來源、地區和頁面是吸引最多人流量的原因。 資料來源為CDN記錄檔或[AEM作業遙測](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/sites/operational-telemetry-for-aem-as-a-cloud-service)。 這兩個來源都有保護隱私權，且不會擷取個人使用者資料。 也有可自訂的篩選器，可協助您調整顯示的資料。

![轉介頁面](/help/dashboards/assets/referral-traffic.png)

此頁面詳細說明下列專案：

* [設定](#setup)
* [篩選器](#filters)
* [整體轉介效能](#overall-performance)
* [熱門轉介URL](#top-referrals)
* [轉介流量詳細資料](#traffic-details)

## 設定 {#setup}

首次登入時，反向連結流量控制面板可能會顯示空白。 若要檢視您的資料，您必須選取&#x200B;**[移至組態]**，以設定轉介流量提供者。

![轉介設定](/help/dashboards/assets/referral-setup1.png)

<!--- 1. Select your Source (either CDN logs or AEM Operational Telemetry).
2. Enter a primary contact email.
3. Click **Request activation** to enable data ingestion. Hiding this until confirmation from PM-->

選取轉介流量提供者後，控制面板會填入轉介流量量度。

## 篩選器 {#filters}

在頁面頂端，您可以套用篩選條件來調整檢視。 您選擇的篩選器將影響儀表板上呈現的&#x200B;**所有**&#x200B;區段。 您可以自訂下列專案：

* **日期範圍** — 選取所顯示資料的時間範圍。 例如最近4週。 您也可選擇選取&#x200B;**自訂周數**&#x200B;選項，以自訂時段。
* **平台** — 選擇特定流量來源，例如Google、OpenAI或社群媒體。
* **頁面意圖** — 依使用者意圖篩選轉介流量。
* **頻道Source** — 依頻道來源篩選。 選項包括：「LLM」、「盈餘」、「付費」或「混合」轉介管道。
* **裝置型別** — 依訪客的裝置型別（桌上型電腦、行動裝置或所有裝置）分析流量。
  **地區** — 檢視不同地理區域的轉介模式。

選取想要的篩選器後，按一下&#x200B;**套用篩選器**，將選取專案套用至儀表板。

## 整體轉介效能 {#overall-performance}

控制面板會顯示關鍵量度，以強調整體轉介效能，包括：

* **總轉介流量** — 來自所有來源的總轉介流量。
* **同意率** — 接受同意提示的訪客百分比。
* **跳出率** — 來自沒有參與事件的轉介來源的工作階段百分比。

![轉介頁面](/help/dashboards/assets/referral-traffic.png)

除了上述的整體績效量度外，**排名最前的區域**&#x200B;面板還會依地理區域劃分流量。 同時，**熱門反向連結來源**&#x200B;面板會顯示帶動最多造訪的平台。 量度的趨勢指標會顯示這些值與上一期間相比隨時間的變化。

<!--## Top Referral URLs {#top-referrals}

The Top Referral URLs list surfaces your site’s most visited pages from referrals.

![Top Referral URLs](/help/dashboards/assets/top-url.png)-->

## 反向連結來源詳細資料和URL效能分析 {#traffic-details}

「反向連結來源詳細資訊」和「URL效能分析」表格可協助您評估流量和品質。 按一下下面的每個標籤以取得詳細資訊：

![轉介流量詳細資料](/help/dashboards/assets/traffic-details.png)

>[!BEGINTABS]

>[!TAB 轉介來源詳細資料]

「反向連結來源詳細資料」檢視會劃分來自不同平台(例如OpenAI、Microsoft、Google和Perplexity)的流量。 它會顯示關鍵量度，例如造訪次數、跳出率和管道型別，協助您瞭解哪些AI和搜尋來源為您的網站帶來最多參與流量。

* **Source** — 轉介流量的來源。
* **造訪** — 每個來源的造訪總數。
* **跳出率** — 來自沒有參與事件的轉介來源的工作階段百分比。
* **管道** — 來源的管道，可以是earned、paid或兩者。

>[!TAB URL效能分析]

「URL效能分析」檢視會根據LLM和其他來源的反向連結流量，將表現最佳的頁面排名。 它會醒目提示流量、跳出率、同意率和頁面意圖等量度，協助您識別哪些頁面吸引並保留AI驅動反向連結中最常參與的訪客。 表格中有一個搜尋欄位，可讓您快速存取主題。

>[!ENDTABS]

在這兩個表格上，您可以使用&#x200B;**匯出**&#x200B;選項來下載表格.csv，並與您的團隊共用深入分析，或在執行報告中包含轉介流量表格。
