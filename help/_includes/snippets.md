---
source-git-commit: b13f91d144d4899198891c4dcd841de8cfbb2355
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 14%

---
# 程式碼片段

## 檢查LLM Optimizer中的路由狀態 {#verify-routing-status-in-ui}

您也可以在 LLM Optimizer 使用者介面中確認流量路由的狀態。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

![將最佳化部署到 AI 代理：已完成](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

## 允許透過防火牆規則在Edge最佳化（選用） {#waf-allowlist-setup}

如果您的CDN使用WAF或機器人管理員：

* 允許列出WAF或機器人管理員中的`*AdobeEdgeOptimize/1.0*`使用者代理程式，讓Edge最佳化服務可以擷取您的來源內容。
* 如果您的防火牆需要使用者代理程式以外的其他驗證，請產生密碼（例如，`openssl rand -hex 32`）並：
   * 將帶有密碼的`x-edgeoptimize-fetcher-key`新增到路由規則中與其他`x-edgeoptimize-*`標頭一起。
   * 新增WAF或機器人管理員規則以允許`x-edgeoptimize-fetcher-key`符合相同密碼的請求。
* 在Edge最佳化會依原樣轉送此標題 — 您擁有完整的金鑰生命週期。

## 返回總覽 {#return-to-overview}

若要進一步瞭解Edge最佳化，包括可用的機會、自動最佳化工作流程和常見問答，請返回[Edge最佳化概覽](/help/dashboards/optimize-at-edge/overview.md)。
