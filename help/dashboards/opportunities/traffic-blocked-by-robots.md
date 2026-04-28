---
title: robots.txt封鎖的流量
description: 瞭解LLM Optimizer如何偵測您的robots.txt檔案何時封鎖AI代理程式存取您的內容，以及如何進行修正。
feature: Opportunities
source-git-commit: fa8b994aa55869cf7f2607e792acc4e8abc213e7
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 1%

---


# robots.txt封鎖的流量

您的`robots.txt`檔案控制哪些爬蟲可以存取您的網站。 當AI代理程式被選擇性地封鎖在一般爬蟲可以存取的內容時，這些代理程式無法索引或引用該內容，直接降低您的品牌在AI產生的回應中的可見度。

Robots.txt所封鎖的流量機會會針對您的熱門頁面分析您的`robots.txt`檔案，並識別會阻止AI代理程式存取他們應該能夠存取的內容的規則。 它會顯示個別`robots.txt`行層級的發現，因此您可以檢閱和更新特定指示，而不是手動稽核整個檔案。

它一眼就能呈現兩個關鍵量度：

- **總URL** — 受您`robots.txt`中封鎖規則影響的URL數目。
- **已封鎖的代理程式** — 被封鎖的AI代理程式數目，無法存取這些URL。

![由robots.txt儀表板封鎖的流量](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-overview.png)

## 運作方式

LLM Optimizer會擷取您的`robots.txt`檔案，並根據6個主要的AI代理程式使用者代理程式檢查您的最上層頁面：

- 克勞德機器人
- GPTBot
- OAI-SearchBot
- OAI-User
- PerplexityBot
- Perplexity-User

只有在萬用字元(`*`)使用者代理程式允許&#x200B;**，但特定AI代理程式**&#x200B;不允許時，才會標示URL。 不會報告總括封鎖（所有爬蟲受到同等限制）。 稽核會特別針對選擇性AI代理程式排除，這代表GEO最可行的發現。

>[!NOTE]
>這個機會不會使用AI來開發或提供建議。 發現完全以您`robots.txt`檔案的直接分析為基礎。

結果會顯示在兩個索引標籤中： **robots.txt**&#x200B;和&#x200B;**代理程式封鎖的流量詳細資料**。

## robots.txt

此索引標籤顯示完整的`robots.txt`檔案，以紅色反白標示封鎖指示。 每個醒目提示的行代表一個規則，該規則會選擇性地阻止一或多個AI代理程式存取一個在其他情況下可公開存取的URL。

![robots.txt檢視中醒目提示區塊指示詞](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-robotstxt.png)

按一下醒目提示的指示詞，會顯示有關其影響和建議修正的詳細資訊。

## 依代理人分類的被封鎖流量詳細資料

此標籤提供AI代理程式所組織的封鎖流量劃分。 對於每個已封鎖的代理程式，它顯示：

- **問題說明** — 說明哪個代理程式遭到封鎖以及它為什麼重要。
- **解決方法** — 開啟`robots.txt`檔案並檢閱每個受影響URL旁邊所列的特定行號的指南。
- 每個已封鎖頁面之受影響URL的資料表，包含&#x200B;**Line**、**Rank**&#x200B;和&#x200B;**URL**。

每個代理程式（例如OAI-User、GPTBot、OAI-SearchBot）都有自己的子標籤，因此您可以處理每個代理程式的區塊。

## 如何修正

若要解決封鎖的代理程式發現，請開啟您的`robots.txt`檔案，並在[依代理程式封鎖的流量詳細資料]索引標籤中，找出每個受影響的URL旁顯示的行號。 更新或移除相關AI代理程式的disallow指令，以允許存取受影響的URL。

例如，若要從特定頁面解除封鎖GPTBot，請移除或更新指令：

```
User-agent: GPTBot
Disallow: /blog/cold-brewing-101
```

更新並重新發佈`robots.txt`檔案後，LLM Optimizer會在下次稽核執行時偵測變更，並將建議標示為已解決。

## 在示範中試用

使用Frescopa示範環境檢視實際操作中的Robots.txt商機所封鎖的流量。

[在Frescopa示範中檢視robots.txt封鎖的流量](https://play.llmo.now/org/demo-org/opportunities/blocked-urls-llm-agents)

## 常見問題

**為何封鎖AI代理程式對GEO很重要？**

產生式引擎最佳化需要AI爬蟲可以存取您的網站內容並編制索引。 封鎖AI代理會直接防止您的頁面出現在AI產生的回應中，減少引文、品牌提及和整體AI可見度。 即使是單一封鎖的高流量頁面，也可能代表大幅降低AI驅動品牌曝光率。

**總括封鎖和選擇性封鎖之間有何差異？**

總括封鎖表示所有爬蟲（包括一般網頁爬蟲）都限制在頁面上。 選擇性封鎖表示一般爬蟲可以存取頁面，但特定的AI代理程式無法存取。 這個機會只會標示選擇性封鎖，因為代表有意或意外將AI代理排除在原本為公開的內容之外，這是最可行的發現。

**LLM Optimizer會檢查哪些AI代理程式？**

LLM Optimizer會檢查ClaudeBot、GPTBot、OAI-SearchBot、OAI-User、PerplexityBot和Perplexity-User。

**如果我刻意封鎖某些AI代理程式怎麼辦？**

您可以檢閱每個已標幟的指示詞，並選擇刻意保留區塊。 已解除的建議會在稽核執行期間保留，除非`robots.txt`檔案變更且規則重新出現，否則將不會重新出現。

**LLM Optimizer如何追蹤一段時間內robots.txt的變更？**

LLM Optimizer使用雜湊來跨執行追蹤您的`robots.txt`內容。 如果先前已解決的封鎖規則重新出現（例如`robots.txt`更新後），它將重新顯示為新的建議。

**如何決定排名最前的頁面？**

頁面來自最高流量SEO頁面、CDN記錄中排名最前的AI代理程式造訪URL以及網站設定中指定的任何自訂URL的組合。
