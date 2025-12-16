---
title: 在Edge最佳化
description: 瞭解如何在CDN邊緣的LLM Optimizer中提供最佳化，而不需要任何編寫變更。
feature: Opportunities
source-git-commit: 39658a057fd4d67f74dc286e1687e384133ac653
workflow-type: tm+mt
source-wordcount: '2224'
ht-degree: 1%

---


# 在Edge最佳化

本頁提供如何在CDN邊緣傳遞最佳化，而不會變更編寫內容的詳細概觀。 內容包括入門流程、可用的最佳化機會，以及如何在Edge自動最佳化。

>[!NOTE]
>此功能目前正在搶先使用。

## 什麼是Edge的最佳化？

Edge最佳化是LLM Optimizer中的邊緣型部署功能，可為LLM使用者代理提供人工智慧易用的變更。 在目前的內容中，「Edge」表示最佳化會套用在CDN層。 由於它在CDN層提供最佳化，因此不需要在內容管理系統(CMS)中進行製作變更，因此您的原始CMS保持不變。 此分隔可讓您在不變更現有發佈工作流程的情況下改善LLM可見性。 它只會鎖定代理程式流量，不影響人類使用者或SEO機器人。 當LLM Optimizer偵測到最佳化頁面的機會時，使用者可以直接在CDN邊緣部署修正。

Edge最佳化是需要複雜工程作業的傳統修正的替代方案，而且速度更快、更精簡。 如前所述，完成一次性設定後，就不需要變更平台或延長開發週期即可套用變更。 您可以在幾分鐘內發佈改善功能，而不需要開發人員的參與。 這是針對AI代理最佳化您的網站的非程式碼方式。

Edge最佳化專為行銷、SEO、內容和數位策略團隊中的業務使用者設計。 這可讓業務使用者完成LLM Optimizer的完整歷程：識別機會、瞭解建議，並輕鬆部署修正。 透過Edge中的最佳化，使用者可以預覽變更、在CDN邊緣快速部署變更，以及驗證最佳化是否即時。 可在LLM Optimizer生態系統中追蹤效能。

### 主要優點

* **僅限AI的傳遞：**&#x200B;僅將最佳化的HTML提供給AI代理程式，不會對人類訪客或SEO機器人造成影響。
* **更快的週期：**&#x200B;在幾分鐘內即可發佈變更，而非數週。 不需要變更平台或延長工程週期。
* **可回覆：**&#x200B;支援一鍵回覆功能，可在數分鐘內回覆頁面。
* **沒有效能影響：**&#x200B;以Edge為基礎的最佳化和快取不會影響網站延遲。
* **CDN與CMS無關：**&#x200B;可與任何CDN設定和前端設定搭配使用，無論內容管理系統為何。

### Edge的最佳化支援哪些機會？

Edge的「最佳化」可支援改善代理程式網頁體驗的商機。 在[機會儀表板](/help/dashboards/opportunities.md)頁面和目前頁面的機會區段中，進一步瞭解每個機會。

## 上線

您應該聯絡您的Adobe客戶團隊或FDE團隊以開始入門流程。 您的IT或CDN團隊也必須完成先決條件和設定程式。 此外，您也可以透過`llmo-at-edge@adobe.com`聯絡我們的團隊以取得進一步的入門協助。

在Edge上線最佳化的先決條件：

* 完成LLM Optimizer的上線流程。
* 完成CDN記錄檔的記錄檔轉送程式。

您的IT/CDN團隊需求：

* 產生API金鑰。
* 在CDN中新增Edge路由規則最佳化。
* 允許列出使用者定義的路徑或整個網域。
* 將使用者定義的LLM使用者代理程式清單新增至目標。
* 請確定`robots.txt`未封鎖任何要鎖定的使用者代理。
* 在LLM Optimizer介面中確認Edge路由最佳化。

以下提供許多CDN設定的設定範例，以指導設定流程。 這些範例應該根據您的實際即時設定進行調整。 我們建議先在較低層級環境中套用變更。

>[!NOTE]
>在以下程式碼範例中，您可能會看到「tokowaka」的參照，這是Edge最佳化的有效專案名稱。 出於相容性的目的，這些識別碼會保留在程式碼中，並參考本檔案中描述的相同功能。

>[!BEGINTABS]

>[!TAB Adobe Managed CDN]

**Adobe Managed CDN**

此設定的目的是使用代理使用者代理程式來設定請求，這些代理使用者代理程式將路由傳送至最佳化處理程式服務（`edge.tokowaka.now`後端）。 若要測試設定，安裝程式完成之後，請在回應中尋找標頭`x-tokowaka-request-id`。

```
curl -svo page.html https://frescopa.coffee/about-us --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-tokowaka-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

路由設定是使用[originSelector CDN規則](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)完成。 先決條件如下：

* 決定要路由的網域
* 決定要路由的路徑
* 決定要路由的使用者代理程式（建議的regex）
* 從Adobe取得`edge.tokowaka.now`後端的api金鑰

若要部署規則，您需要：

* 建立[設定管道](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/operations/config-pipeline)
* 認可存放庫中的`cdn.yaml`設定檔
* 將api金鑰部署為[秘密環境變數](https://experienceleague.adobe.com/zh-hant/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)
* 執行設定管道


```
kind: "CDN"
version: "1"
data:
  # Origin selectors to route to Tokowaka backend
  originSelectors:
    rules:
      - name: route-to-tokowaka-backend
        when:
          allOf:
            - reqHeader: x-tokowaka-request
              exists: false # avoid loops when requests comes from Tokowaka
            - reqHeader: user-agent
              matches: "(?i)(Tokowaka-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)" # routed user agents
            - reqProperty: domain
              equals: "example.com" # routed domain
            - reqProperty: originalPath
              matches: '(/[^./]+|\.html|/)$' # routed extensions, with .html extension or without extension
            - anyOf:
              - { reqProperty: originalPath, in: [ "/page.html" ] } # routed pages, exact path matching
              - { reqProperty: originalPath, like: "/dir/*" } # routed pages, wildcard path matching
        action:
          type: selectOrigin
          originName: tokowaka-backend
          headers:
            x-tokowaka-api-key: "${{TOKOWAKA_API_KEY}}"
    origins:
      - name: tokowaka-backend
        domain: "edge.tokowaka.now"
```

若要測試設定，請執行curl並預期出現以下情況：

```
curl -svo page.html https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-tokowaka-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

<!-- >>[!TAB Akamai (BYOCDN)]

**Tokowaka BYOCDN - Akamai**

```
{
    "name": "Project Tokowaka CDN Rule",
    "children": [
        {
            "name": "Connection settings",
            "children": [],
            "behaviors": [
                {
                    "name": "advanced",
                    "options": {
                        "description": "",
                        "xml": "<forward:availability.health-detect.status>off</forward:availability.health-detect.status>\n<forward:availability>\n<max-reforwards>1</max-reforwards>\n<max-reconnects>1</max-reconnects>\n</forward:availability>\n<match:forward.server-type value=\"CUSTOMER_ORIGIN\">\n<network:http.read>%(PMUSER_HTTP_READ)</network:http.read>\n<network:http.first-byte-timeout>%(PMUSER_FIRST_BYTE_TIMEOUT)</network:http.first-byte-timeout>\n<network:http.connect-timeout>%(PMUSER_HTTP_CONNECT_TIMEOUT)</network:http.connect-timeout> \n</match:forward.server-type>"
                    },
                    "uuid": "4a8c027b-1b23-44a7-8e12-f8d07e453679",
                    "templateUuid": "41c77091-419f-43f2-9a84-0b406b050cc8"
                }
            ],
            "uuid": "4759571b-8036-4c16-9b60-d3aeb84958f1",
            "criteria": [],
            "criteriaMustSatisfy": "all"
        },
        {
            "name": "Site Failover Behavior",
            "children": [],
            "behaviors": [
                {
                    "name": "failAction",
                    "options": {
                        "actionType": "RECREATED_CO",
                        "contentCustomPath": false,
                        "contentHostname": "www.adobe.com",
                        "enabled": true
                    }
                },
                {
                    "name": "advanced",
                    "options": {
                        "description": "",
                        "xml": "<forward:availability.fail-action2>\n<add-header>\n<status>on</status>\n<name>x-tokowaka-request</name>\n<value>fo</value>\n</add-header>\n</forward:availability.fail-action2>"
                    }
                }
            ],
            "uuid": "b3000c12-1ab8-49b1-a5d0-75e58bb18c9c",
            "criteria": [
                {
                    "name": "matchResponseCode",
                    "options": {
                        "lowerBound": 400,
                        "matchOperator": "IS_BETWEEN",
                        "upperBound": 599
                    }
                },
                {
                    "name": "originTimeout",
                    "options": {
                        "matchOperator": "ORIGIN_TIMED_OUT"
                    }
                }
            ],
            "criteriaMustSatisfy": "any",
            "comments": "If Tokowaka origin returns a 4xx or 5xx error (or times out), failover condition is to send the request back to Akamai and set the x-tokowaka-request header so we don't re-send the request to Tokowaka"
        }
    ],
    "behaviors": [
        {
            "name": "origin",
            "options": {
                "cacheKeyHostname": "ORIGIN_HOSTNAME",
                "compress": true,
                "customValidCnValues": [
                    "{{Origin Hostname}}",
                    "{{Forward Host Header}}",
                    "*.tokowaka.now"
                ],
                "enableTrueClientIp": true,
                "forwardHostHeader": "ORIGIN_HOSTNAME",
                "hostname": "edge.tokowaka.now",
                "httpPort": 80,
                "httpsPort": 443,
                "ipVersion": "IPV4",
                "minTlsVersion": "DYNAMIC",
                "originCertificate": "",
                "originCertsToHonor": "STANDARD_CERTIFICATE_AUTHORITIES",
                "originSni": true,
                "originType": "CUSTOMER",
                "ports": "",
                "standardCertificateAuthorities": [
                    "akamai-permissive",
                    "THIRD_PARTY_AMAZON"
                ],
                "tlsVersionTitle": "",
                "trueClientIpClientSetting": true,
                "trueClientIpHeader": "True-Client-IP",
                "verificationMode": "CUSTOM"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_LLMCLIENT",
                "variableValue": "TRUE"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "caseSensitive": false,
                "extractLocation": "CLIENT_REQUEST_HEADER",
                "globalSubstitution": false,
                "headerName": "Accept-Language ",
                "regex": "^([a-zA-Z]{2}).*",
                "replacement": "$1",
                "transform": "SUBSTITUTE",
                "valueSource": "EXTRACT",
                "variableName": "PMUSER_LANG"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_X_FORWARDED_HOST",
                "variableValue": "{{builtin.AK_HOST}}"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_TOKOWAKA_CACHE_KEY",
                "variableValue": "LLMCLIENT={{user.PMUSER_LLMCLIENT}};LANG={{user.PMUSER_LANG}};X_FORWARDED_HOST={{user.PMUSER_X_FORWARDED_HOST}}"
            }
        },
        {
            "name": "caching",
            "options": {
                "behavior": "CACHE_CONTROL_AND_EXPIRES",
                "cacheControlDirectives": "",
                "defaultTtl": "1d",
                "enhancedRfcSupport": false,
                "honorMustRevalidate": false,
                "honorPrivate": false,
                "mustRevalidate": false
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "X-tokowaka-api-key",
                "newHeaderValue": "<your api-key here>",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "x-tokowaka-config",
                "newHeaderValue": "LLMCLIENT={{user.PMUSER_LLMCLIENT}};LANG={{user.PMUSER_LANG}}",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "x-tokowaka-url",
                "newHeaderValue": "{{builtin.AK_URL}}",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "cacheId",
            "options": {
                "rule": "INCLUDE_VARIABLE",
                "variableName": "PMUSER_TOKOWAKA_CACHE_KEY"
            }
        },
        {
            "name": "modifyIncomingResponseHeader",
            "options": {
                "action": "DELETE",
                "customHeaderName": "Age",
                "standardDeleteHeaderName": "OTHER"
            }
        },
        {
            "name": "prefreshCache",
            "options": {
                "enabled": true,
                "prefreshval": 90
            }
        },
        {
            "name": "modifyOutgoingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "X-Forwarded-Host",
                "newHeaderValue": "{{builtin.AK_HOST}}",
                "standardModifyHeaderName": "OTHER"
            }
        }
    ],
    "criteria": [
        {
            "name": "userAgent",
            "options": {
                "matchCaseSensitive": false,
                "matchOperator": "IS_ONE_OF",
                "matchWildcard": true,
                "values": [
                    "*Tokowaka-AI*",
                    "*ChatGPT-User*",
                    "*GPTBot*",
                    "*OAI-SearchBot*"
                ]
            }
        },
        {
            "name": "path",
            "options": {
                "matchCaseSensitive": false,
                "matchOperator": "MATCHES_ONE_OF",
                "normalize": false,
                "values": [
                ]
            }
        },
        {
            "name": "requestHeader",
            "options": {
                "headerName": "x-tokowaka-request",
                "matchOperator": "DOES_NOT_EXIST",
                "matchWildcardName": false
            }
        },
        {
            "name": "matchVariable",
            "options": {
                "matchCaseSensitive": true,
                "matchOperator": "IS",
                "matchWildcard": false,
                "variableExpression": "FALSE",
                "variableName": "PMUSER_TOKOWAKA_DISABLE"
            }
        }
    ],
    "criteriaMustSatisfy": "all"
}
```

Important considerations:

* Tokowaka Rule will be ON based on User-Agent + Path + x-tokowaka-request (if not present) + TOKOWAKA_DISABLE variable (to allow switch off using a single variable toggle)
* Set up rules to **Modify Incoming Request Headers** rule to set custom headers
* Set cache-key in Akamai using user defined variable through Cache-ID modification mechanism. Only a single user defined variable is allowed, so create a separate variable for cache_key and set it accordingly.
* Lang: extracted from Accept-Language header using "regex": "^([a-zA-Z]{2}).*"
* With Cache ID Modification within a match on User Agent, the content can't be purged by URL (just FYI)
* Site failover mechanism: With the match on User-Agent rule, Akamai does not allows to failover based on health check, but only only basis of origin response/connectivity per request. Set **x-tokowaka-fo:true**  resp header in case of failover response.
* SWR is not supported by Akamai. So, only TTL based caching is there. So, configure a rule in Akamai to strip Age header from origin response else TTL based caching would not work.
* Ensure that the Tokowaka rule is the bottom most rule in the rule hierarchy (so that it overrides all other rules).-->

>[!TAB Fastly (BYOCDN)]

**Tokowaka BYOCDN - Fastly - VCL**

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![新增VCL程式碼片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv程式碼片段**

```
unset req.http.x-tokowaka-url;
unset req.http.x-tokowaka-config;
unset req.http.x-tokowaka-api-key;

if (!req.http.x-tokowaka-request
    && req.http.user-agent ~ "(?i)(Tokowaka-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-fowarded-host = req.http.host; # required for identifying the original host
  set req.http.x-tokowaka-url = req.url; # required for identifying the original url
  set req.http.x-tokowaka-config = "LLMCLIENT=true"; # required for cache key
  set req.http.x-tokowaka-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_Tokowaka;
}
```

**vcl_hash程式碼片段**

```
if (req.http.x-tokowaka-config) {
  set req.hash += "tokowaka";
  set req.hash += req.http.x-tokowaka-config;
}
```

**vcl_deliver程式碼片段**

```
if (req.http.x-tokowaka-config && resp.status >= 400) {
  set req.http.x-tokowaka-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-tokowaka-config && req.http.x-tokowaka-request == "failover") {
  set resp.http.x-tokowaka-fo = "1";
}
```

>[!ENDTABS]

>[!NOTE]
>若是其他CDN提供者，請聯絡`llmo-at-edge@adobe.com`以協助您的IT/CDN團隊完成入門。 完成設定後，您可以在LLM Optimizer中部署最佳化Edge機會的建議。

## 機會

下表顯示可改善一般網頁體驗並受Edge最佳化支援的機會。

| 機會 | 類型 | 自動識別 | 自動建議 | 自動最佳化 |
|---------|----------|----------|----------|----------|
| 復原內容可見性 | 技術地理位置 | 偵測對AI代理程式隱藏關鍵內容的頁面。 顯示受影響的URL和可復原的預期內容。 | 強調指出可供AI代理使用的內容，並建議啟用這些頁面的預先呈現。 | 提供完整演算、適合AI使用的HTML快照，以代理流量復原先前隱藏的內容。 |
| 最佳化LLM的標題 | 內容最佳化 | 掃描標題以偵測空的、重複的、遺漏的或模稜兩可的標題，這會降低機器可讀性。 | 建議更簡潔的標題階層和改良的標籤，並顯示每個頁面更新結構的預覽。 | 插入AI代理程式的改良標題結構，保留您的視覺設計，同時讓LLM更容易讀取頁面。 |
| 新增LLM易記摘要 | 內容最佳化 | 會識別在頁面或區段層級缺乏簡要摘要的長頁面或複雜頁面，讓AI難以快速掃描和理解。 | 建議在擷取關鍵內容的頁面和區段層級，使用AI產生的簡短摘要。 | 將摘要插入相關的HTML區段，改善模型解譯和說明頁面內容的方式。 |
| 新增相關常見問答 | 內容最佳化 | 偵測現有頁面內容中可能受益於常見問答集的意圖差距。 | 根據使用者意圖和現有主題建議AI產生的常見問題集內容。 | 將常見問題集內容插入HTML，讓頁面在AI導向的答案中更易發現且相關。 |
| 簡化複雜內容 | 內容最佳化 | 標示可能妨礙AI理解的具有複雜文字的頁面。 | 提供AI產生的複雜文字的簡化版本，同時保留原始含義。 | 重寫頁面中的複雜區段，改善AI可讀性。 |

### 其他工具

[Adobe LLM Optimizer：您的網頁是否可編輯？](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome擴充功能可讓您確切瞭解LLM可存取的網頁內容量，以及隱藏的專案。 此工具是免費獨立診斷工具，不需要產品授權或設定。

只要按一下，您就可以評估任何網站的電腦可讀性。 您可以檢視AI代理程式檢視的內容與人類使用者檢視內容的並排比較，並估計使用LLM Optimizer可復原的內容量。 請參閱[AI是否可以讀取您的網站？](https://business.adobe.com/blog/introducing-the-llm-optimizer-chrome-extension)頁以取得詳細資訊。

## 機會詳細資料

在接下來的章節中，您可以檢視Edge最佳化支援的每個商機的其他詳細資料。

### 復原內容可見性

此機會會標籤因使用者端轉譯而為AI代理隱藏關鍵內容的頁面。 對於每個已識別的頁面，它都會精確顯示AI代理程式檢視中缺少哪些內容、醒目提示可見度差距，並可讓您直接套用變更以復原隱藏的內容。 當您透過Edge的「最佳化」部署此商機時，系統會為LLM使用者代理程式提供預先轉譯的AI最佳化頁面版本，讓他們無需執行Javascript即可存取完整內容。
這可確保頁面首先對AI代理完全可見。 在該預先轉譯的HTML上套用其他增強功能。

>[!IMPORTANT]
>此預先呈現功能在Edge部署最佳化時自動套用至下面顯示的所有機會，以確保AI代理程式完全可看見頁面。

### 最佳化LLM的標題

此機會會偵測標題結構導致AI代理程式因標題空白、重複、遺失或模稜兩可而難以理解頁面的頁面。 對於每個受影響的頁面，機會會顯示次優標題，並建議更清晰的階層。 透過Edge的「最佳化」進行部署時，改良的標題會套用至代理流量的HTML。 這樣有助於機器可讀性，同時保持面向人類的版面配置不變。

### 新增LLM易記摘要

這個機會可辨識哪些頁面可受益於簡潔的摘要，以協助LLM快速瞭解頁面內容的內容。 對於每個頁面，機會會偵測最需要摘要的位置，並在頁面層級或區段層級建立AI產生的摘要。 使用Edge最佳化進行部署時，這些摘要會插入AI代理程式擷取的HTML中，進而提高更準確地描述您內容的機會。

### 新增相關常見問答

此機會會標籤頁面，指出其他問答內容可以更好地匹配使用者意圖和AI驅動探索中的提示。 對於每個頁面，它會建議與頁面上的使用者意圖和內容相連結的AI產生的常見問題集區塊。 透過Edge最佳化，這些常見問題集會插入到HTML中，讓您的頁面更適合AI，並增加AI回答直接反映您指引的可能性。

### 簡化複雜內容

這個機會會尋找具有長且複雜段落的頁面，這些段落可能會降低AI理解。 對於超過可讀性臨界值的每個頁面，它會建立AI產生的內容，這些內容更簡單、更易於掃描，同時保留原始含義。 部署於邊緣時，傳送至代理流量的簡化內容可協助LLM更忠實地解讀及摘要您的內容。

## 在Edge自動最佳化

您可以針對每個機會，在邊緣預覽、編輯、部署、檢視即時和復原最佳化。

### 預覽

**預覽**&#x200B;可讓您在建議上線之前檢視建議的影響。 它會顯示目前頁面與套用建議後預期的AI最佳化版本之間的並排差異。 此檢視使用與支援即時流量的Edge最佳化邏輯相同的邏輯，但處於隔離的預覽模式。 這不會影響即時流量，因為這是供檢閱的唯讀模擬。

![預覽](/help/assets/optimize-at-edge/preview.png)

### 編輯

**編輯**&#x200B;可讓您在部署自動產生的建議之前，先調整或完全重寫這些建議。 您不必接受建議，而是透過編輯工作流程保持完全控制權。 檢視會在結構化編輯器中顯示提議的變更，您可以在此修改文字以更符合您的原始意圖。 部署後，編輯的版本將傳送給AI代理程式。

![編輯](/help/assets/optimize-at-edge/edit.png)

### 部署

**部署**&#x200B;會發佈選取的建議，以便從邊緣提供最佳化的體驗給AI代理程式。 如果CDN已完全路由，則網域中的所有頁面通常會在數分鐘內隨著新變更上線。 如果已針對選取的路徑設定路由，則只有列入允許清單的頁面會啟動最佳化。

![部署](/help/assets/optimize-at-edge/deploy.png)

### 檢視即時

**檢視即時**&#x200B;可讓您驗證最佳化是否即時，並如預期般執行一般流量，否則將難以存取此檢視。 您可以在「固定建議」下檢視即時頁面，這會將頁面呈現給AI代理程式。

![即時檢視](/help/assets/optimize-at-edge/view-live.png)

### 復原

復原可安全地回覆先前部署的最佳化。 僅限AI的頁面版本通常會在數分鐘內恢復其先前的狀態，以便您視需要安全地嘗試最佳化。

![回覆](/help/assets/optimize-at-edge/rollback.png)

## 常見問題

問：Edge的「最佳化」會鎖定哪些型別的LLM？

要鎖定的使用者代理清單由您在上線流程中定義。

<!--Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.-->

問：如果我還未加入Edge的最佳化，會發生什麼事？

如果您在完成必要的設定之前按一下&#x200B;**部署最佳化**，將不會有任何內容套用至您的網站。 反之，快顯對話方塊會提示您透過`llmo-at-edge@adobe.com`聯絡我們的團隊以尋求入門協助。 在上線完成之前，您仍然可以探索偵測到的機會和建議，但一鍵式部署工作流程將保持非使用中。

問：在來源更新內容時會發生什麼事？

只要基礎來源頁面未變更，我們就會從快取中提供頁面的最佳化版本。 不過，當來源確實變更時，我們的系統會自動重新整理，讓AI代理程式一律會收到最新的內容。 這是因為我們使用低快取存留時間(TTL)設定（依分鐘數順序），因此您網站上的任何內容更新都會觸發該視窗中的新最佳化。 由於沒有適合每個網站的通用TTL，因此我們可以根據您的快取失效規則來設定此TTL，以確保兩個系統保持同步。

問：Edge最佳化是否僅適用於使用Adobe Edge Delivery Service (EDS)的網站？

不行。在Edge中最佳化與CDN無關，且適用於任何前端架構，而不僅僅是Adobe的EDS棧疊上部署的架構。

問：Edge預先轉譯的「最佳化」與傳統伺服器端轉譯(SSR)有何不同？

兩者都可以解決不同的問題，並共同作業。 傳統SSR會呈現伺服器端內容，但不包含稍後在瀏覽器中載入的內容。 在Edge預先呈現時最佳化會在JavaScript和使用者端資料載入後擷取頁面，在CDN邊緣產生完整組合的版本。 SSR著重於改善人類體驗，而Edge最佳化則能改善LLM的網頁體驗。
