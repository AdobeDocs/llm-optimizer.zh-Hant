---
title: 邊緣最佳化：CloudFront (BYOCDN)
description: 了解在 LLM Optimizer 中如何設定 CloudFront BYOCDN 進行邊緣最佳化。
feature: Opportunities
source-git-commit: 1253d0f0a58f6523699c52fbfab23028dc469c82
workflow-type: ht
source-wordcount: '2228'
ht-degree: 100%

---


# CloudFront (BYOCDN)

此設定會將代理式流量 (來自 AI 機器人和 LLM 使用者代理的要求) 路由至 Edge Optimize 後端服務 (`live.edgeoptimize.net`)。 真人訪客和 SEO 機器人仍照常由您的來源伺服器提供服務。 若要測試設定，在完成設定之後，請於回應中尋找 `x-edgeoptimize-request-id` 標頭。

**先決條件**

在設定 CloudFront 之前，請確認您具備：

* 為您的網站提供服務的現有 CloudFront 分發
* AWS IAM 的權限，可建立 Lambda 函式、IAM 角色、CloudFront 分發及快取原則。
* 已完成 LLM Optimizer 上線流程。
* 已經將內容傳遞網路記錄轉送至 LLM Optimizer。
* 從 LLM Optimizer 使用者介面擷取的 Edge Optimize API 金鑰。

{{retrieve-byocdn-api-key}}

**步驟 1：建立 Edge Optimize 來源**

**導覽：**「AWS 主控台」>「CloudFront」>「分發」>「[您的分發]」>「來源」標籤

1. 按一下「**建立來源**」。

2. 設定來源：
   * **來源網域：**`live.edgeoptimize.net`
   * **名稱：**`EdgeOptimize_Origin`

3. 所有其他欄位皆保留其預設值。

4. 新增自訂標頭：

   | 頁首 | 值 |
   |--------|-------|
   | `x-edgeoptimize-api-key` | 您的 API 金鑰 |
   | `x-forwarded-host` | `www.example.com` |

   將 `www.example.com` 替換成真實的網站網域，並將 `Your API key` 替換成 Adobe 代表提供給您的 Edge Optimize API 金鑰。

5. 按一下「**建立來源**」。

![建立 Cloudfront 來源](/help/assets/optimize-at-edge/cloudfront-origin-creation.png)

**步驟 2：建立檢視器請求函式**

**導覽：**「AWS 主控台」>「CloudFront」>「函式」

1. 按一下「**建立函式**」。

2. 設定：
   * **名稱：**`edgeoptimize-routing`
   * **執行階段：**`cloudfront-js-2.0`

3. 將預設程式碼替換成取自 [viewer-request.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/cloudfront-function/viewer-request.js) 的程式碼。

   發佈前，請自訂程式碼中的下列值：

   * `YOUR_DEFAULT_ORIGIN`：替換成您現有的預設來源名稱 (名稱可在「CloudFront」>「分發」>「[您的分發]」>「來源」標籤中找到)。
   * `TARGETED_PATHS`：設定為 `null` 可鎖定所有 HTML 頁面，或設定為特定路徑的陣列，例如 `['/', '/products', '/about']`。

4. 按一下「**儲存變更**」>「**發佈函式**」。

![建立 Cloudfront 函式](/help/assets/optimize-at-edge/cloudfront-function-creation.png)


**步驟 3：設定快取原則**

**導覽：**「AWS 主控台」>「CloudFront」>「分發」>「[您的分發]」>「行為」

檢查目前套用至您的行為的快取原則。在您的行為上按一下「**編輯**」，然後查看「**快取鍵和來源請求**」區段，以確認您的情境：

* **情境 A (舊版)：**&#x200B;您會看到已選取標示為「**舊版快取設定**」的選項按鈕。沒有任何原則名稱下拉式選單，而是會看到內嵌 TTL 和標頭設定。
* **情境 B (自訂原則)：**&#x200B;您會看到已選取&#x200B;**快取原則**，而且該原則使用由您或您的團隊建立的名稱 (非 AWS 提供的原則)。
* **情境 C (受管理原則)：**&#x200B;您會看到已選取&#x200B;**快取原則**，而且該原則使用 AWS 提供的名稱，例如 `CachingOptimized`、`CachingDisabled` 或 `CachingOptimizedForUncompressedObjects`，且這些名稱無法編輯。

**情境 A：舊版快取設定**

如果您的行為使用舊版快取設定：

1. 在「**快取鍵和來源請求**」下方，您會看到已選取「**舊版快取設定**」。

2. 將 `x-edgeoptimize-config` 和 `x-edgeoptimize-url` 新增至「**標頭**」允許清單：

   * 從下拉式選單中選取「**包含下列標頭**」。
   * 新增「`x-edgeoptimize-config`」和「`x-edgeoptimize-url`」。
     ![Cloudfront 快取舊版](/help/assets/optimize-at-edge/cloudfront-cache-policy-legacy.png)

   如果您已在「標頭」下拉式選單中選取「**全部**」，請略過此步驟，因為所有標頭都會自動轉送至來源。

3. 檢查「**物件快取**」設定：

   * 如果設定為「**自訂**」，建議將「**最小 TTL**」設定為「`0`」。不過，如果您目前的「最小 TTL」已經很短，則可能不需要變更。
   * 如果設定為「**使用來源快取標頭**」，則不需要任何變更。

4. 按一下「**儲存變更**」。

**情境 B：具有自訂快取原則的非舊版設定**

如果您的行為已使用自訂快取原則 (由您建立而非 AWS 管理的原則)：

**導覽：**「AWS 主控台」>「CloudFront」>「原則」>「快取」

1. 按一下您的現有原則。

2. 按一下「**編輯**」。

3. 建議將「**最小 TTL**」設定為「`0`」。不過，如果您目前的「最小 TTL」已經很短，則可能不需要變更。
   ![快取原則 TTL 設定](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

4. 在「**快取鍵設定**」>「**標頭**」下方，連同您現有的包含項目，新增「`x-edgeoptimize-config`」和「`x-edgeoptimize-url`」。
   ![快取原則標頭](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

5. 按一下「**儲存變更**」。

**情境 C：具有受管理之 (AWS) 快取原則的非舊版設定**

如果您的行為使用 AWS 管理的快取原則 (例如 `CachingOptimized`)，就無法加以編輯。您必須建立新的自訂快取原則來複製現有的受管理原則設定，並在上方新增 Edge Optimize 標頭。

**第 1 部分：記下您目前的受管理快取原則設定**

**導覽：**「AWS 主控台」>「CloudFront」>「原則」>「快取」

1. 尋找並按一下目前附加至您的行為的受管理快取原則 (例如 `CachingOptimized`)。

2. 記下所有現有設定：
   * 最小 TTL、最大 TTL、預設 TTL
   * 包含在快取鍵中的標頭
   * 包含在快取鍵中的 Cookie
   * 包含在快取鍵中的查詢字串
   * 壓縮支援 (Gzip、Brotli)

**第 2 部分：使用相同的設定加上 Edge Optimize 設定來建立新的自訂快取原則**

**導覽：**「AWS 主控台」>「CloudFront」>「原則」>「快取」

1. 按一下「**建立快取原則**」。

2. **名稱：**`edgeoptimize-cache`

   ![快取原則名稱](/help/assets/optimize-at-edge/cloudfront-cache-policy-name.png)

3. 複製在第 1 部分記下的所有設定，並修改以下部分：

   * 建議將「**最小 TTL**」設定為「`0`」。不過，如果您目前的「最小 TTL」已經很短，則可能不需要變更。

   ![快取原則 TTL 設定](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

   * 在「**快取鍵設定**」>「**標頭**」下方，包含受管理原則具備的所有內容，並新增「`x-edgeoptimize-config`」和「`x-edgeoptimize-url`」。

   ![快取原則標頭](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

4. 按一下「**建立**」。

5. 回到您的行為並與新建立的原則建立關聯：

   **導覽：**「AWS 主控台」>「CloudFront」>「分發」>「[您的分發]」>「行為」

   1. 編輯您的行為。
   2. 在「**快取鍵和來源請求**」下方，選取「**快取原則**」。
   3. 從下拉式選單中選擇「`edgeoptimize-cache`」。
   4. 按一下「**儲存變更**」。

**步驟 4：建立 Lambda@Edge 函式 (來源請求與回應)**

>[!IMPORTANT]
>Lambda@Edge 函式&#x200B;**必須在 `us-east-1` (維吉尼亞州北部) 區域**&#x200B;建立。這是 AWS 的要求。即使是在 `us-east-1` 中建立函式，AWS 會自動將函式複製到全球所有 CloudFront 邊緣位置，以便在距離檢視器最近的邊緣位置執行函式。繼續執行之前，請確認您位於 AWS 主控台中的 `us-east-1` 區域。

**建立 Lambda 函式**

**導覽：**「AWS 主控台」>「Lambda」

1. 按一下「**建立函式**」。

2. 選取「**從頭開始製作**」。

3. 設定：
   * **函式名稱：**`edgeoptimize-origin`
   * 所有其他欄位皆保留其預設值。

4. 按一下「**建立函式**」。

5. 在程式碼編輯器中，將預設程式碼替換成取自 [origin-request-response.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/origin-request-response.js) 的程式碼。

6. 按一下「**部署**」以儲存程式碼。

7. 記下「**設定**」>「**權限**」下方顯示的&#x200B;**執行角色名稱** (例如 `edgeoptimize-origin-role-xxxxx`)。在接下來的步驟中，您會需要此名稱。

**更新執行角色的信任原則**

自動建立的角色只會信任 `lambda.amazonaws.com`。針對 Lambda@Edge，您也必須新增「`edgelambda.amazonaws.com`」。

**導覽：**「AWS 主控台」>「IAM」>「角色」>「[您在上一步的角色]」>「信任關係」分頁

1. 按一下「**編輯信任原則**」。

2. 將該原則替換成取自 [trust-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/trust-policy.json) 的內容。

3. 按一下「**更新原則**」。

>[!WARNING]
>Lambda@Edge **必須**&#x200B;設定服務主體 `edgelambda.amazonaws.com`。若沒有服務主體，CloudFront 就無法在邊緣位置叫用您的函式。

**修正 CloudWatch 記錄權限原則**

自動建立的角色隨附針對一般 Lambda 設定的 `AWSLambdaBasicExecutionRole` 原則，而該原則的 Lambda@Edge 區域和記錄群組名稱皆不正確。您必須加以更新。

**導覽：**「AWS 主控台」>「IAM」>「角色」>「[您的角色]」>「權限」標籤 > 按一下附加的原則名稱 (例如 `AWSLambdaBasicExecutionRole-xxxx`)

1. 按一下「**編輯**」。

2. 將該原則替換成取自 [cloudwatch-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/cloudwatch-policy.json) 的內容。

   在 JSON 中，將 `ACCOUNT_ID` 替換成您實際的 AWS 帳戶 ID (可在 AWS 主控台的右上角找到)，並將 `FUNCTION_NAME` 替換成您的 Lambda 函式名稱 (例如 `edgeoptimize-origin`)。

3. 按一下「**儲存變更**」。

>[!WARNING]
>ARN 中的區域必須是 `*`，Lambda@Edge 會在離檢視器最近的邊緣位置執行，因此記錄會寫入邊緣位置所在區域的 CloudWatch (例如 `ap-south-1`、`eu-west-1`)，而不一定是在 `us-east-1`。記錄群組會使用冠上區域前置詞的名稱：`/aws/lambda/us-east-1.FUNCTION_NAME`，其中 `us-east-1` 永遠是函式的主區域。

**發佈版本**

1. 在函式頁面上，按一下「**動作**」(右上方) >「**發佈新版本**」。

2. 新增說明。

3. 點擊&#x200B;**發佈**。
   ![Lambda 發佈](/help/assets/optimize-at-edge/cloudfront-lambda-publish.png)

4. 複製或記下&#x200B;**函式 ARN**，在下一個步驟會需要使用。
   ![Lambda ARN](/help/assets/optimize-at-edge/cloudfront-lambda-arn.png)

**步驟 5：將函式和快取原則與行為建立關聯**

**導覽：**「AWS 主控台」>「CloudFront」>「分發」>「[您的分發]」>「行為」

1. 編輯您的行為。

2. 如果您在步驟 3 (情境 C) 中建立了新的快取原則，請將「**快取原則**」設定為「`edgeoptimize-cache`」。
   ![快取原則](/help/assets/optimize-at-edge/cloudfront-behaviour-cache.png)

3. 在「**函式關聯**」下方進行下列設定：

   * **檢視器請求：**`edgeoptimize-routing`
   * **來源請求：**&#x200B;來自步驟 4 的版本化函式 ARN (在「**發佈版本**」中)
   * **來源回應：**&#x200B;來自步驟 4 的版本化函式 ARN (在「**發佈版本**」中)

   ![函式關聯設定](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 按一下「**儲存變更**」。

**步驟 6：測試設定**

**1. 測試機器人流量 (應經過最佳化)**

使用代理式機器人使用者代理傳送請求。在&#x200B;**第一個請求**&#x200B;上，Edge Optimize 在處理及快取頁面時，可能會傳回 Proxy 處理的 (非最佳化) 回應。您可以透過回應中的 `x-edgeoptimize-proxy: 1` 標頭加以識別。

運用代理式使用者代理模擬 AI 機器人要求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的回應會包含 `x-edgeoptimize-request-id` 標頭，確認要求已經透過 Edge Optimize 進行路由：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 測試真人流量 (不應受到影響)**

模擬一般真人瀏覽器要求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

回應&#x200B;**不應**&#x200B;包含 `x-edgeoptimize-request-id` 標頭。 頁面內容和回應時間應與啟用邊緣最佳化之前維持相同。

**3. 如何區分這兩種情境**

| 頁首 | 機器人流量 (最佳化) | 真人流量 (不受影響) |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在：包含唯一的要求 ID | 不存在 |
| `x-edgeoptimize-fo` | 唯有發生容錯移轉時存在 (值：`1`) | 不存在 |

您也可以在 LLM Optimizer 使用者介面中確認流量路由的狀態。 導覽至「**客戶設定**」，然後選取「**內容傳遞網路設定**」標籤。

![已啟用路由的 AI 流量路由狀態](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

**4. 檢查記錄是否正常傳輸**

執行上述測試請求後，請確認 CloudFront 函式和 Lambda@Edge 函式的記錄皆正常寫入。

*CloudFront 函式記錄 (`edgeoptimize-routing`)*

**導覽：**「AWS 主控台」>「CloudWatch」>「記錄群組」(在 `us-east-1` 或設定 CloudFront 分發的區域中)

1. 尋找名稱為 `/aws/cloudfront/function/edgeoptimize-routing` 的記錄群組。
2. 開啟最新的記錄資料流。
3. 若是代理式請求，您應該會看到類似下列項目：
   * `Adding origin group for userAgent: chatgpt-user`
   * `Routing to Edge Optimize origin for userAgent: chatgpt-user`
4. 若是非代理式請求，您應該會看到：
   * `Routing to Default origin for userAgent: ...`

您也可以查看&#x200B;**「AWS 主控台」>「CloudFront」>「函式」>「edgeoptimize-routing」**&#x200B;下方的「**度量**」標籤，以檢視叫用計數和錯誤率。

*Lambda@Edge 記錄 (`edgeoptimize-origin`)*

>[!IMPORTANT]
>Lambda@Edge 記錄會寫入處理請求之&#x200B;**邊緣位置所在區域**&#x200B;的 CloudWatch 中，而非 `us-east-1`。請檢查與您執行 curl 指令之位置最接近之 AWS 區域中的 CloudWatch。

**導覽：**「AWS 主控台」>「CloudWatch」>「記錄群組」(確保您位於正確的區域)

1. 尋找名稱為 `/aws/lambda/us-east-1.edgeoptimize-origin` 的記錄群組。
2. 開啟最新的記錄資料流。
3. 若是代理式請求，您應該會看到類似下列項目：
   * `Calling Edge Optimize Origin for agentic requests`：主要路徑
   * `Calling Default Origin in case of failover for agentic requests`：容錯移轉路徑
   * `Failover Triggered for agentic requests`：來源與回應容錯移轉偵測

如果沒有記錄群組，請確認是否已在步驟 4 中正確地更新 IAM 權限。也請檢查附近的其他 AWS 區域，因為處理您的請求的邊緣位置可能與預期不同。

**疑難排解**

| 問題 | 可能的原因 | 解決方案 |
|-------|----------------|----------|
| 代理式請求的回應中沒有 `x-edgeoptimize-request-id` | 來源群組未路由至 Edge Optimize | 確認 CloudFront 函式程式碼中已正確地替換 `YOUR_DEFAULT_ORIGIN` (步驟 2)。 |
| 代理式請求出現 403 錯誤 | API 金鑰無效或遺失 | 檢查 Edge Optimize 來源自訂標頭中的 `x-edgeoptimize-api-key` 標頭值 (步驟 1)。 |
| 找不到 Lambda@Edge 的 CloudWatch 記錄 | IAM 權限錯誤 | 確認 CloudWatch 記錄的權限原則已更新 (步驟 4)。注意：Lambda@Edge 記錄會出現在處理請求的邊緣區域中，而不一定是在 `us-east-1`。 |
| 快取未執行 `cache-control: no-store` | 最小 TTL 可能太高 | 將快取原則中的「最小 TTL」設定為「`0`」(步驟 3)。如果「最小 TTL」已經很短，這可能不是問題所在。 |
| 一般 (非代理式) 流量在設定後中斷 | 快取原則設定錯誤 | 如果您建立了新的快取原則 (情境 C)，請確定您已複製原始受管理原則的所有設定。 |

**停用和重新啟用邊緣最佳化**

Lambda@Edge 函式 (`edgeoptimize-origin`) 與 CloudFront 行為的來源請求和來源回應事件相關聯。因為該函式會針對透過該行為 (包括真人和代理式) 的每個請求以內嵌方式執行，所以 Lambda@Edge 運作中斷將影響所有即時流量，而不只是代理式請求。如果您偵測到 Lambda@Edge 運作中斷，請立即移除函式關聯，以便讓正常流量還原至預設來源。

**如何偵測 Lambda@Edge 運作中斷**

* **AWS 服務健康狀態儀表板**：檢查「[AWS 服務健康狀態儀表板](https://health.aws.amazon.com/health/status)」是否有影響 **Amazon CloudFront** 或 **AWS Lambda** 的任何現行事件。這裡報告的全域或區域性運作中斷，是確認問題發生在 AWS 基礎架構端而非在您設定中的最快方式。
* **Lambda@Edge 錯誤**：導覽至&#x200B;**「AWS 主控台」>「CloudFront」>「監視」>「[您的分發]**」。開啟「**Lambda@Edge 錯誤**」標籤並檢查「**執行錯誤**」圖表，找出是否發生執行錯誤。如果這些值很高，Lambda@Edge 可能已停機。

**解除 Lambda@Edge 函式的關聯**

**導覽：**「AWS 主控台」>「CloudFront」>「分發」>「[您的分發]」>「行為」

1. 在您的行為上按一下「**編輯**」。

2. 向下捲動至「**函式關聯**」區段。

3. 將下列關聯設定為「**無關聯**」：

   | 事件 | 變更為 |
   |---|---|
   | 檢視器請求 | 無關聯 |
   | 來源請求 | 無關聯 |
   | 來源回應 | 無關聯 |

   ![函式關聯設定](/help/assets/optimize-at-edge/cloudfront-no-function-association.png)

4. 按一下「**儲存變更**」。

5. 等待 CloudFront 分發完成部署。狀態通常會在幾分鐘內從「**部署中**」變更為上次修改日期。

完成部署後，所有流量都會直接路由到預設來源。不會刪除任何設定；Lambda 函式及其關聯隨時可以還原。

**重新關聯 Lambda@Edge 函式**

**導覽：**「AWS 主控台」>「CloudFront」>「分發」>「[您的分發]」>「行為」

1. 在您的行為上按一下「**編輯**」。

2. 向下捲動至「**函式關聯**」區段。

3. 還原關聯：

   | 事件 | 設定為 |
   |---|---|
   | 檢視器請求 | `edgeoptimize-routing` (CloudFront 函式) |
   | 來源請求 | 取自步驟 4 的版本化 Lambda ARN |
   | 來源回應 | 取自步驟 4 的版本化 Lambda ARN |

   使用您在步驟 4 記下的版本化&#x200B;**函式 ARN** (在「**發佈版本**」中)。

   ![函式關聯設定](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 按一下「**儲存變更**」。

5. 等候分發完成部署，然後確認代理式請求傳回 `x-edgeoptimize-request-id` 標頭 (如步驟 6 所述)。

{{return-to-overview}}
