---
title: 在Edge最佳化 — CloudFront (BYOCDN)
description: 瞭解如何在LLM Optimizer中為Edge最佳化設定CloudFront BYOCDN。
feature: Opportunities
source-git-commit: 1253d0f0a58f6523699c52fbfab23028dc469c82
workflow-type: tm+mt
source-wordcount: '2228'
ht-degree: 1%

---


# CloudFront (BYOCDN)

此設定會將代理流量（來自AI機器人和LLM使用者代理的請求）路由到Edge最佳化後端服務(`live.edgeoptimize.net`)。 系統仍照常從您的來源提供人類訪客和SEO機器人。 若要測試設定，安裝完成之後，請在回應中尋找標頭`x-edgeoptimize-request-id`。

**先決條件**

在設定CloudFront設定之前，請確定您擁有：

* 為網站提供服務的現有CloudFront散發。
* 建立Lambda函式、IAM角色、CloudFront分配和快取原則的AWS IAM許可權。
* 已完成LLM Optimizer上線程式。
* 已完成CDN記錄檔轉送至LLM Optimizer。
* 從Edge UI擷取的LLM Optimizer最佳化API金鑰。

{{retrieve-byocdn-api-key}}

**步驟1：建立Edge最佳化來源**

**導覽：** AWS Console > CloudFront >分配> [您的分配] >來源標籤

1. 按一下&#x200B;**建立來源**。

2. 設定來源：
   * **原始網域：** `live.edgeoptimize.net`
   * **名稱：** `EdgeOptimize_Origin`

3. 將所有其他欄位保留為其預設值。

4. 新增自訂標頭：

   | 頁首 | 值 |
   |--------|-------|
   | `x-edgeoptimize-api-key` | 您的API金鑰 |
   | `x-forwarded-host` | `www.example.com` |

   將`www.example.com`取代為您的實際網站網域，並將`Your API key`取代為您的Edge代表提供的Adobe最佳化API金鑰。

5. 按一下&#x200B;**建立來源**。

![Cloudfront來源建立](/help/assets/optimize-at-edge/cloudfront-origin-creation.png)

**步驟2：建立檢視器要求函式**

**導覽：** AWS Console > CloudFront >函式

1. 按一下&#x200B;**建立函式**。

2. 設定：
   * **名稱：** `edgeoptimize-routing`
   * **執行階段：** `cloudfront-js-2.0`

3. 將預設程式碼取代為[viewer-request.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/cloudfront-function/viewer-request.js)的程式碼。

   發佈前，請自訂程式碼中的下列值：

   * `YOUR_DEFAULT_ORIGIN` — 將取代為您現有的預設來源名稱（位於CloudFront > Distributions > [Your Distribution] > Originates索引標籤中）。
   * `TARGETED_PATHS` — 設定為`null`以鎖定所有HTML頁面，或設定為特定路徑的陣列，例如`['/', '/products', '/about']`。

4. 按一下&#x200B;**儲存變更** > **發佈函式**。

![建立Cloudfront函式](/help/assets/optimize-at-edge/cloudfront-function-creation.png)


**步驟3：設定快取原則**

**導覽：** AWS Console > CloudFront >分佈> [您的分佈] >行為

檢查目前附加至您行為的快取原則。 按一下您的行為上的&#x200B;**編輯**，並檢視&#x200B;**快取金鑰和來源要求**&#x200B;區段以識別您的案例：

* **案例A （舊版）：**&#x200B;您看到標示為&#x200B;**已選取舊版快取設定**&#x200B;的選項按鈕。 沒有原則名稱下拉式清單 — 而是會看到內嵌TTL和標題設定。
* **案例B （自訂原則）：**&#x200B;您看到選取的&#x200B;**快取原則**&#x200B;是您或您的團隊建立的原則名稱(不是AWS提供的原則)。
* **案例C （受管理原則）：**&#x200B;您看到以AWS提供的名稱（例如`CachingOptimized`、`CachingDisabled`或`CachingOptimizedForUncompressedObjects`）選取了&#x200B;**快取原則** — 這些無法編輯。

**案例A：舊版快取設定**

如果您的行為使用舊版快取設定：

1. 在&#x200B;**快取金鑰和原始要求**&#x200B;底下，您將會看到&#x200B;**已選取的舊版快取設定**。

2. 將`x-edgeoptimize-config`和`x-edgeoptimize-url`新增至&#x200B;**Headers**&#x200B;允許清單：

   * 從下拉式清單中選取&#x200B;**包含下列標頭**。
   * 新增`x-edgeoptimize-config`和`x-edgeoptimize-url`。
     ![舊版Cloudfront快取](/help/assets/optimize-at-edge/cloudfront-cache-policy-legacy.png)

   如果您已在「標頭」下拉式清單中選取&#x200B;**全部**，請略過此步驟 — 所有標頭會自動轉送至來源。

3. 檢查&#x200B;**物件快取**&#x200B;設定：

   * 若設為&#x200B;**自訂** — 建議將&#x200B;**最小TTL**&#x200B;設為`0`。 不過，如果您目前的最低TTL已經非常短，您可能不需要加以變更。
   * 若設為&#x200B;**使用原始快取標題** — 不需要變更。

4. 按一下&#x200B;**儲存變更**。

**案例B：具有自訂快取原則的非舊式**

如果您的行為已使用自訂快取原則(由您建立的原則，而非AWS管理的原則)：

**導覽：** AWS Console > CloudFront >原則>快取

1. 按一下您現有的原則。

2. 按一下「**編輯**」。

3. 建議將&#x200B;**最小TTL**&#x200B;設定為`0`。 不過，如果您目前的最低TTL已經非常短，您可能不需要加以變更。
   ![快取原則TTL設定](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

4. 在&#x200B;**快取金鑰設定** > **標頭**&#x200B;以及您現有的包含專案下，新增`x-edgeoptimize-config`和`x-edgeoptimize-url`。
   ![快取原則標頭](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

5. 按一下&#x200B;**儲存變更**。

**案例C：非舊版的Managed (AWS)快取原則**

如果您的行為使用AWS Managed快取原則（例如，`CachingOptimized`），則無法加以編輯。 您需要建立新的自訂快取原則，以複製現有的受管理原則設定，並在上方新增Edge Optimize標頭。

**第1部分：記下您目前的Managed快取原則設定**

**導覽：** AWS Console > CloudFront >原則>快取

1. 尋找並按一下目前附加至您行為的受管理快取原則（例如，`CachingOptimized`）。

2. 記下所有現有設定：
   * 最小TTL、最大TTL、預設TTL
   * 快取金鑰中包含的標頭
   * 快取金鑰中包含的Cookie
   * 快取索引鍵中包含的查詢字串
   * 壓縮支援(Gzip、Brotli)

**第2部分：使用相同的設定建立新的自訂快取原則+ Edge最佳化設定**

**導覽：** AWS Console > CloudFront >原則>快取

1. 按一下&#x200B;**建立快取原則**。

2. **名稱：** `edgeoptimize-cache`

   ![快取原則名稱](/help/assets/optimize-at-edge/cloudfront-cache-policy-name.png)

3. 複製第1部分所述的所有設定，並作下列修改：

   * 建議將&#x200B;**最小TTL**&#x200B;設定為`0`。 不過，如果您目前的最低TTL已經非常短，您可能不需要加以變更。

   ![快取原則TTL設定](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

   * 在&#x200B;**快取金鑰設定** > **標頭**&#x200B;下，包含受管理原則的所有內容，加上新增`x-edgeoptimize-config`和`x-edgeoptimize-url`。

   ![快取原則標頭](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

4. 按一下「**建立**」。

5. 返回您的行為並關聯新建立的原則：

   **導覽：** AWS Console > CloudFront >分佈> [您的分佈] >行為

   1. 編輯您的行為。
   2. 在&#x200B;**快取金鑰和原始要求**&#x200B;下，選取&#x200B;**快取原則**。
   3. 從下拉式清單中選擇`edgeoptimize-cache`。
   4. 按一下&#x200B;**儲存變更**。

**步驟4：建立Lambda@Edge函式（原始要求與回應）**

>[!IMPORTANT]
>必須在`us-east-1` （維吉尼亞北部）區域&#x200B;**中建立Lambda@Edge函式**。 這是AWS的需求。 即使函式是在`us-east-1`中建立，AWS也會自動將其複製到全球所有CloudFront邊緣位置，因此它會在距離檢視器最近的邊緣位置執行。 繼續之前，請確定您位於AWS主控台的`us-east-1`區域。

**建立Lambda函式**

**導覽：** AWS主控台> Lambda

1. 按一下&#x200B;**建立函式**。

2. 從頭開始選取&#x200B;**作者**。

3. 設定：
   * **函式名稱：** `edgeoptimize-origin`
   * 將所有其他欄位保留為其預設值。

4. 按一下&#x200B;**建立函式**。

5. 在程式碼編輯器中，將預設程式碼取代為[origin-request-response.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/origin-request-response.js)的程式碼。

6. 按一下&#x200B;**部署**&#x200B;以儲存程式碼。

7. 請注意&#x200B;**組態** > **許可權**&#x200B;下方顯示的&#x200B;**執行角色名稱** （例如`edgeoptimize-origin-role-xxxxx`）。 您需要在以下步驟中完成此步驟。

**更新執行角色的信任原則**

自動建立的角色只信任`lambda.amazonaws.com`。 針對Lambda@Edge，您也必須新增`edgelambda.amazonaws.com`。

**導覽：** AWS Console > IAM >角色> [您在上一步驟中的角色] >信任關係索引標籤

1. 按一下&#x200B;**編輯信任原則**。

2. 以[trust-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/trust-policy.json)的內容取代原則。

3. 按一下&#x200B;**更新原則**。

>[!WARNING]
>Lambda@Edge的`edgelambda.amazonaws.com`服務主體為&#x200B;**必要**。 若沒有它，CloudFront就無法在邊緣位置叫用您的函式。

**修正CloudWatch記錄檔許可權原則**

自動建立的角色隨附為一般Lambda設定的`AWSLambdaBasicExecutionRole`原則，該原則具有錯誤的Lambda@Edge區域和記錄群組名稱。 您需要更新它。

**導覽：** AWS Console > IAM >角色> [您的角色] >許可權索引標籤>按一下附加的原則名稱（例如，`AWSLambdaBasicExecutionRole-xxxx`）

1. 按一下「**編輯**」。

2. 以[cloudwatch-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/cloudwatch-policy.json)中的內容取代原則。

   在JSON中，將`ACCOUNT_ID`取代為您的實際AWS帳戶ID (可在AWS主控台的右上角找到)，並將`FUNCTION_NAME`取代為您的Lambda函式的名稱（例如，`edgeoptimize-origin`）。

3. 按一下&#x200B;**儲存變更**。

>[!WARNING]
>ARN中的區域必須是`*` — Lambda@Edge會在離檢視器最近的邊緣位置執行，因此記錄檔會寫入邊緣位置區域的CloudWatch （例如，`ap-south-1`、`eu-west-1`），不一定是`us-east-1`。 記錄群組使用區域首碼名稱： `/aws/lambda/us-east-1.FUNCTION_NAME`，其中`us-east-1`永遠是函式的主區域。

**發佈版本**

1. 在函式頁面上，按一下&#x200B;**動作** （右上方） > **發佈新版本**。

2. 新增說明。

3. 點擊&#x200B;**發佈**。
   ![Lambda發佈](/help/assets/optimize-at-edge/cloudfront-lambda-publish.png)

4. 複製或記下&#x200B;**函式ARN** — 您需要在下一個步驟中執行此操作。
   ![Lambda ARN](/help/assets/optimize-at-edge/cloudfront-lambda-arn.png)

**步驟5：將函式和快取原則與行為關聯**

**導覽：** AWS Console > CloudFront >分佈> [您的分佈] >行為

1. 編輯您的行為。

2. 如果您在步驟3 （案例C）中建立新的快取原則，請將&#x200B;**快取原則**&#x200B;設定為`edgeoptimize-cache`。
   ![快取原則](/help/assets/optimize-at-edge/cloudfront-behaviour-cache.png)

3. 在&#x200B;**函式關聯**&#x200B;下，設定：

   * **檢視器要求：** `edgeoptimize-routing`
   * **原始要求：**&#x200B;來自步驟4的版本化函式ARN （在&#x200B;**發佈版本**）
   * **原始回應：**&#x200B;來自步驟4的版本化函式ARN （在&#x200B;**發佈版本**）

   ![函式關聯組態](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 按一下&#x200B;**儲存變更**。

**步驟6：測試設定**

**1. 測試機器人流量（應該最佳化）**

使用代理程式機器人使用者代理程式傳送請求。 在&#x200B;**第一個請求**&#x200B;上，Edge Optimize在處理並快取頁面時可能會傳回代理（非最佳化）回應。 您可以透過回應中的`x-edgeoptimize-proxy: 1`標題識別此專案。

使用代理使用者代理程式模擬AI機器人請求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的回應包含`x-edgeoptimize-request-id`標頭，確認要求已透過Edge最佳化路由：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 測試人力流量（不應受影響）**

模擬一般的人類瀏覽器請求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

回應應&#x200B;**不**&#x200B;包含`x-edgeoptimize-request-id`標頭。 在Edge中啟用「最佳化」之前，頁面內容和回應時間應保持相同。

**3. 如何區分這兩個案例**

| 頁首 | 機器人流量（最佳化） | 人類流量（未受影響） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在 — 包含唯一的請求識別碼 | 不存在 |
| `x-edgeoptimize-fo` | 只有在發生容錯移轉時才會出現（值： `1`） | 不存在 |

流量路由的狀態也可以在LLM Optimizer UI中檢視。 瀏覽至&#x200B;**客戶組態**&#x200B;並選取&#x200B;**CDN組態**&#x200B;標籤。

已啟用路由的![AI流量路由狀態](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

**4. 驗證記錄檔是否正確流動**

執行上述測試請求後，確認已針對CloudFront函式和Lambda@Edge函式寫入記錄。

*CloudFront函式記錄(`edgeoptimize-routing`)*

**導覽：** AWS Console > CloudWatch >記錄群組（在`us-east-1`中或設定CloudFront發佈的區域）

1. 尋找名為`/aws/cloudfront/function/edgeoptimize-routing`的記錄群組。
2. 開啟最新的記錄資料流。
3. 對於代理請求，您應該會看到類似以下的專案：
   * `Adding origin group for userAgent: chatgpt-user`
   * `Routing to Edge Optimize origin for userAgent: chatgpt-user`
4. 若為非代理式請求，您應會看到：
   * `Routing to Default origin for userAgent: ...`

您還可以檢查&#x200B;**AWS Console > CloudFront > Functions > edgeoptimize-routing**&#x200B;下的&#x200B;**Metrics**&#x200B;索引標籤，以檢視叫用計數和錯誤率。

*Lambda@Edge記錄檔(`edgeoptimize-origin`)*

>[!IMPORTANT]
>Lambda@Edge記錄檔會寫入至服務請求之邊緣位置&#x200B;**的**&#x200B;區域中的CloudWatch，而非`us-east-1`。 在距離您執行curl命令最近的AWS區域中檢查CloudWatch。

**導覽：** AWS Console > CloudWatch >記錄群組（確定您位於正確的區域）

1. 尋找名為`/aws/lambda/us-east-1.edgeoptimize-origin`的記錄群組。
2. 開啟最新的記錄資料流。
3. 對於代理請求，您應該會看到類似以下的專案：
   * `Calling Edge Optimize Origin for agentic requests` — 主要路徑
   * `Calling Default Origin in case of failover for agentic requests` — 容錯移轉路徑
   * `Failover Triggered for agentic requests` — 來源 — 回應容錯移轉偵測

如果記錄群組不存在，請確認已在步驟4中正確更新IAM許可權。 另請檢查附近的其他AWS區域 — 提供您請求的邊緣位置可能與您預期的不同。

**疑難排解**

| 問題 | 可能的原因 | 解決方案 |
|-------|----------------|----------|
| 沒有回應代理要求的`x-edgeoptimize-request-id` | 來源群組未路由至Edge最佳化 | 確認CloudFront函式程式碼中的`YOUR_DEFAULT_ORIGIN`已正確取代（步驟2）。 |
| 代理請求出現403錯誤 | API金鑰無效或遺失 | 檢查Edge最佳化原始自訂標題中的`x-edgeoptimize-api-key`標題值（步驟1）。 |
| 找不到Lambda@Edge的CloudWatch記錄 | 錯誤的IAM許可權 | 確認CloudWatch記錄檔許可權原則已更新（步驟4）。 注意： Lambda@Edge記錄檔會出現在為請求提供服務的邊緣區域中，不一定是`us-east-1`。 |
| 快取未接受`cache-control: no-store` | 最小TTL可能太高 | 將快取原則中的最小TTL設定為`0` （步驟3）。 如果最低TTL已經很短，這可能不是問題。 |
| 一般（非代理）流量在設定後中斷 | 快取原則設定錯誤 | 如果您已建立新的快取原則（案例C），請確定您已複製原始受管理原則的所有設定。 |

**在Edge停用並重新啟用最佳化**

Lambda@Edge函式(`edgeoptimize-origin`)與您的CloudFront行為的原始要求與原始回應事件相關聯。 因為它會內嵌執行每個傳遞該行為（包括人類和代理程式）的請求，所以Lambda@Edge中斷將會影響所有即時流量，而不僅僅是代理程式請求。 如果您偵測到Lambda@Edge中斷，請立即移除函式關聯，以將正常流量還原至您的預設來源。

**如何偵測Lambda@Edge中斷**

* **AWS服務健康情況儀表板** — 檢查[AWS服務健康情況儀表板](https://health.aws.amazon.com/health/status)，瞭解影響&#x200B;**Amazon CloudFront**&#x200B;或&#x200B;**AWS Lambda**&#x200B;的任何作用中事件。 這裡所報告的全域或區域中斷是確認問題位於AWS基礎架構側（而不是您的設定）的最快方式。
* **Lambda@Edge錯誤** — 瀏覽至&#x200B;**AWS Console > CloudFront >監視> [您的發佈]**。 開啟&#x200B;**Lambda@Edge錯誤**&#x200B;標籤，並檢查&#x200B;**執行錯誤**&#x200B;圖形的執行錯誤。 如果這些值很高，Lambda@Edge可能會關閉。

**分離Lambda@Edge函式**

**導覽：** AWS Console > CloudFront >分佈> [您的分佈] >行為

1. 按一下您行為上的&#x200B;**編輯**。

2. 向下捲動至&#x200B;**函式關聯**&#x200B;區段。

3. 將下列關聯設定為&#x200B;**無關聯**：

   | 事件 | 變更為 |
   |---|---|
   | 檢視器請求 | 無關聯 |
   | 來源請求 | 無關聯 |
   | 來源回應 | 無關聯 |

   ![函式關聯組態](/help/assets/optimize-at-edge/cloudfront-no-function-association.png)

4. 按一下&#x200B;**儲存變更**。

5. 等待CloudFront散發完成部署。 狀態會從&#x200B;**部署**&#x200B;變更為上次修改日期，通常在幾分鐘內。

部署後，所有流量都會直接路由到您的預設來源。 不會刪除任何設定；Lambda函式及其關聯可隨時還原。

**重新附加Lambda@Edge函式**

**導覽：** AWS Console > CloudFront >分佈> [您的分佈] >行為

1. 按一下您行為上的&#x200B;**編輯**。

2. 向下捲動至&#x200B;**函式關聯**&#x200B;區段。

3. 還原關聯：

   | 事件 | 將設為 |
   |---|---|
   | 檢視器請求 | `edgeoptimize-routing` （CloudFront函式） |
   | 來源請求 | 步驟4的版本化Lambda ARN |
   | 來源回應 | 步驟4的版本化Lambda ARN |

   使用您在步驟4 （在&#x200B;**發佈版本**）中注意到的版本化&#x200B;**函式ARN**。

   ![函式關聯組態](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 按一下&#x200B;**儲存變更**。

5. 等候發佈完成部署，然後確認代理程式要求會傳回`x-edgeoptimize-request-id`標頭，如步驟6所述。

{{return-to-overview}}
