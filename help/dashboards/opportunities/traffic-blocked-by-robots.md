---
title: 遭 robots.txt 封鎖的流量
description: 了解 LLM Optimizer 如何偵測 robots.txt 檔案是否阻止 AI 代理存取您的內容，以及如何修正這些問題。
feature: Opportunities
autotag-review: '2026-07-15T18:04:23.113Z'
TQID: 'https://experienceleague.adobe.com/rk82xEtvYBr47tTNzhC6ZbFw1Bim3Otr-D2ncBWPmwM'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
subfeature_v2: id: e0ec491f-fe51-42b6-801c-1c0dfcc0e64f
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: b69b2659-1057-424e-8fc5-ed9e016dc554
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e1e0219c-f879-479f-8427-888ed2a6e9c2id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 817
ht-degree: 100%

---


# 遭 robots.txt 封鎖的流量

您的 `robots.txt` 檔案控制哪些爬蟲可以存取您的網站。 當 AI 代理被選擇性封鎖，無法存取一般爬蟲可存取的內容時，這些代理便無法建立內容索引或引用該內容，這會直接降低品牌在 AI 生成回答中的能見度。

「遭 robots.txt 封鎖的流量」機會根據您的熱門頁面分析 `robots.txt` 檔案，並找出阻止 AI 代理存取原本應可存取內容的規則。 此機會針對`robots.txt` 每一行顯示分析結果，讓您審閱並更新特定指令，而不必手動稽核整個檔案。

此機會以一目了然的方式顯示兩項關鍵量度：

- **URL 總數**：受到 `robots.txt` 封鎖規則影響的 URL 數量。
- **遭封鎖的代理**：遭封鎖而無法存取這些 URL 的 AI 代理數量。

![遭 robots.txt 封鎖的流量儀表板](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-overview.png)

## 運作方式

LLM Optimizer 會擷取您的 `robots.txt` 檔案，並使用六種主要 AI 代理使用者代理來檢查您的熱門頁面：

- ClaudeBot
- GPTBot
- OAI-SearchBot
- OAI-User
- PerplexityBot
- Perplexity-User

只有當 URL **對萬用字元 (`*`) 使用者代理開放，但封鎖特定 AI 代理時，才會標示該 URL**。 系統不會回報全面封鎖的情況，即所有爬蟲皆受到相同限制的情況。 此稽核會特別針對選擇性封鎖 AI 代理的情況，因為這類問題最具生成式引擎最佳化處理價值。

>[!NOTE]
>此機會不會使用 AI 來產生或提供建議。 結果完全來自對您 `robots.txt` 檔案的直接分析。

結果會顯示在兩個標籤中：**「robots.txt」** 和&#x200B;**「依 AI 代理分類的遭封鎖流量詳細資訊」**。

## robots.txt

此分頁標籤會顯示完整的 `robots.txt` 檔案，並以紅色標示封鎖指令。 醒目標示的每一行代表一項規則，該規則會選擇性封鎖一個或多個 AI 代理，使其無法存取其他可公開存取的 URL。

![醒目標示封鎖指令的 robots.txt 視圖](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-robotstxt.png)

按一下醒目標示的指令，即可查看其影響與建議修正的詳細資訊。

## 依代理人分類的被封鎖流量詳細資料

此分頁標籤會提供依 AI 代理分類的封鎖流量之細項解析。 每個遭封鎖的 AI 代理會顯示以下項目：

- **問題說明**：說明哪些 AI 代理遭封鎖以及這個情況為何需要重視。
- **解決方法**：指引您開啟 `robots.txt` 檔案，並審閱每個受影響 URL 旁所列的特定行號。
- 受影響 URL 的表格，包含每個遭封鎖頁面的&#x200B;**行**、**排名**&#x200B;和 **URL**。

每個 AI 代理 (例如 OAI-User、GPTBot、OAI-SearchBot) 都有自己的子分頁標籤，所以您可以針對每個代理處理封鎖問題。

## 如何修正

若要解決 AI 代理遭封鎖的問題，請開啟您的 `robots.txt` 檔案，並在「依 AI 代理分類的遭封鎖流量詳細資訊」分頁標籤中，找到每個受影響 URL 旁顯示的行號。 更新或移除相關 AI 代理的禁止指令，以允許存取受影響的 URL。

例如，若要允許 GPTBot 存取特定頁面，請移除或更新下列指令：

```
User-agent: GPTBot
Disallow: /blog/cold-brewing-101
```

更新並重新發佈 `robots.txt` 檔案後，LLM Optimizer 會在下次稽核執行時偵測變更，並將建議標示為已解決。

## 在示範中試用

使用 Frescopa 示範環境，查看「遭 robots.txt 封鎖的流量」機會的實際運作情形。

[在 Frescopa 示範中檢視「遭 robots.txt 封鎖的流量」](https://play.llmo.now/org/demo-org/opportunities/blocked-urls-llm-agents)

## 常見問題

**為何封鎖 AI 代理會影響生成式引擎最佳化？**

生成式引擎最佳化需要 AI 爬蟲可以存取您的網站內容並建立索引。 封鎖 AI 代理會直接導致您的頁面無法出現在 AI 生成的回答中，進而降低引用次數、品牌提及次數和整體 AI 能見度。 即使只有單一高流量頁面遭到封鎖，也可能大幅降低 AI 驅動的品牌曝光度。

**全面封鎖和選擇性封鎖之間有何差異？**

全面封鎖表示所有爬蟲 (包括一般網頁爬蟲) 都無法存取某個頁面。 選擇性封鎖表示一般爬蟲可以存取該頁面，但特定 AI 代理無法存取。 此機會只會標示選擇性封鎖，因為這代表有意或無意將 AI 代理排除於原本公開的內容之外，而這類問題最具處理價值。

**LLM Optimizer 會檢查哪些 AI 代理？**

LLM Optimizer 會檢查 ClaudeBot、GPTBot、OAI-SearchBot、OAI-User、PerplexityBot 和 Perplexity-User。

**如果我想刻意封鎖某些 AI 代理，該怎麼做？**

您可以審閱每個已標示的指令，並選擇保留該封鎖設定。 在多次稽核執行期間，已解除的建議均會保留但不會重新顯示，除非 `robots.txt` 檔案變更且規則重新出現。

**LLM Optimizer 如何追蹤 robots.txt 的歷時變更？**

LLM Optimizer 會使用雜湊，在多次執行間追蹤您的 `robots.txt` 內容。 如果先前已解決的封鎖規則重新出現 (例如 `robots.txt` 更新後)，系統會將其重新顯示為新的建議。

**如何判定熱門頁面？**

頁面來源結合您的高流量搜尋引擎最佳化頁面、內容傳遞網路記錄中 AI 代理最常造訪的 URL，以及網站設定中指定的任何自訂 URL。
