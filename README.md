# Web-Application-Security-Testing-Note

[![copyright © 2020 By MksYi](https://img.shields.io/badge/copyright%20©-%202020%20By%20MksYi-blue.svg)](https://mks.tw/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

首先這是根據 [OWASP Testing Guide v4 Table of Contents](https://www.owasp.org/index.php/OWASP_Testing_Guide_v4_Table_of_Contents) 的 [Web Application Security Testing](https://www.owasp.org/index.php/Web_Application_Penetration_Testing) 理，基本上官方提供的驗證方法，除了 OWASP TOP 10 上的弱點之外，還概括 Web Application 上所有已知、常見的弱點，項目非常的多。

## 前言

資訊安全其實是一個很日常，且很廣泛的議題，當朋友問起什麼是資訊安全時，我總會這樣做介紹，安全是定義在現有的事物之上，有行車，就有行車安全，有程式，就有程式安全，現今資訊無所無在，你我生活中所常用的電腦、手機，甚至也不僅限於走在電路上的 0 與 1，在你腦中的那些小秘密，對於他人來說可能都是寶貴的資訊，而這些資訊該如何妥善管理？這就是資訊安全探討的範疇。

若你不是資安技術人員，想從生活的角度去了解資安，可以參考「[生活資安五四三！從生活周遭看風險與資訊安全～番外篇](https://ithelp.ithome.com.tw/users/20013608/ironman/2539)」一文，這一系列的文章，從日常生活中提出與資安有關地議題並加以討論，大多都是你我早已知道，卻選擇性忽視的議題。

而該章節所探討的網頁應用程式（Web App）也僅僅只佔了資安議題的一部分而已，至於延伸還可以涵蓋其他串接的服務、作業系統、硬體、等…，而網頁只是個常見的窗口，為了打穿主機拿控制權，接著看是否可以成為跳板，提權、維持權限，接著攻擊其他主機或是服務，來達到內網滲透的目的，又稱網域滲透，包過去打 [Vulnhub](https://www.vulnhub.com/) 上的靶機，也都是為因應工作需要滲透能力而做的練習與準備。

延伸閱讀：「Tags: [資訊安全/滲透測試/vulnhub](https://mks.tw/category/資訊安全/滲透測試/vulnhub)」

## Start Testing

其實一直以來，我都很想針對 CTF 題目來寫一篇對應到 OWASP TOP 10 的文章，雖說這只是冰山一角，自從開始工作後，才明白真正的 PT 所需的能力是非常全面的，包含對網路、網域、各種程式語言、作業系統環境都需要一定程度的認識，資安果然是一個很可怕的坑，講到這好像有些離題了，拉回來繼續探討 Web Application 的測試項目，如下。

1.  [Information Gathering](#Information-Gathering)
2.  [Identity Management Testing](#Identity-Management-Testing)
3.  [Configuration and Deployment Management Testing](#Configuration-and-Deployment-Management-Testing)
4.  [Identity Management Testing](#Identity-Management-Testing)
5.  [Authentication Testing](#Authentication-Testing)
6.  [Authorization Testing](#Authorization-Testing)
7.  [Session Management Testing](#Session-Management-Testing)
8.  [Input Validation Testing](#Input-Validation-Testing)
9.  [Testing for Error Handling](#Testing-for-Error-Handling)
10.  [Testing for weak Cryptography](#Testing-for-weak-Cryptography)
11.  [Business Logic Testing](#Business-Logic-Testing)
12.  [Client Side Testing](#Client-Side-Testing)

依照順序檢視，第一件事情就是情報蒐集，搭配情報分析的結果，了解使用什麼語言、服務、套件，含有什麼樣的特性，是否使用包含一些含有已知弱點的服務、套件，或是分析哪些地方可以輸入，隱含哪些弱點存在，接著再針對服務上的功能進行測試，在 Testing Guite 的文件上，就提供這麼一系列的服務測試方法。

測試項目也分成許多不同面向，有些項目的定義非常相近，卻又分別歸類在多個不同的類別之中，官方大致上分成 12 個類別，從服務設定上的問題，概括到不安全的程式撰寫，或是忽視一些語言及服務特性而造成的系統危安，只要含有一定程度的弱點存在，整個服務就岌岌可危，這也是所謂的「木桶理論」，意旨防禦就橡木桶周圍的木條，而最短邊的木條就決定整個系統的安全性。

![壁纸资源网 http://www.oucal.com/detail/aHR0cHM6Ly90aW1nc2EuYmFpZHUuY29tL3RpbWc%23_%23_%23aW1hZ2UmcXVhbGl0eT04MCZzaXplPWI5OTk5XzEwMDAwJnNlYz0xNTc4MDI2OTUyNDA4JmRpPWI3NjQwNzkyZTBiZTUzYWYzY2U0NWM3YjZlNTFiMDg1JmltZ3R5cGU9MCZzcmM9aHR0cCUzQSUyRiUyRmkwLmhkc2xiLmNvbSUyRmJmcyUyRmFydGljbGUlMkY3Y2UyMGM2MDU5MWI1ZDQwZWI0ZTIxZGRkMTFlMmViNmY4MmQ4NTM4LmpwZ0BfQF9A6L6p6K%2BB55qE5pyo5qG255CG6K66QF9AX0DmnKjmobbnn63mnb%23_%23_%23nkIborrrlo4Hnurg%3D.html](https://i.imgur.com/IkU74XB.png)

## Information Gathering

蒐集情報在滲透測試是相當重要的環節，相對影響的是如何採取攻擊與決策，若是不了解你的敵人，也就絲毫不知道該從哪裡開始下手，更不用說想攻破對方，情蒐有很多種方式，藉由各種服務、語言都包含一些特別可辨識之處，這邊稱為 Fingerprint（指紋）。

![](https://i.imgur.com/ySCaYBA.png)

Service Fingerprint 可以透過 Header、Meta、Cookie 來判斷，為求效率，可以透過工具 Wappalyzer 來輔助蒐集情資。

![](https://i.imgur.com/Fv2SgUT.png)

關於網站地圖、路徑分佈，也可以使用 Zed Attack Proxy (ZAP) 來自動化的蒐集路徑資料，此外 ZAP 也會簡單分析各頁面上的可能風險。

![](https://i.imgur.com/rufawHx.png)

除此之外，還可以借助 Google 大神的力量，該技術被稱作 Google Hacking，透過 `site:"mks.tw" 後台` 就可以針對 `mks.tw` 這個 Domain 找尋包含 `後台` 的頁面，當然還有其他用法，有興趣的可以自行參考 [Google Hacking Database (GHDB)](https://www.exploit-db.com/google-hacking-database)。

其次也可以從 robots.txt 等網頁爬蟲的相關配置上找到一些資訊，robots.txt 是網頁上針對搜尋引擎爬蟲所提出的聲明，可以制定於哪些路徑上的網站可否被收錄，通常管理者若不希望搜尋引擎可以找到管理頁面、後台，就會把後台路徑寫在 robots.txt 上，藉此反其道而行來蒐集情報。

![Robots.txt 包含敏感路徑](https://i.imgur.com/juP8EIU.png)

## Configuration and Deployment Management Testing

這部分要測試的是系統上的服務是否安全，安全的系統搭配不安全的設定也會造成危害，例如預設的 apache 服務，預設有目錄瀏覽，若是沒有設定關閉，可能會有 Information Leak 的問題，比較誇張的還有網站上線後，連帶 `.git` 都包在裡面，並且沒有任何的禁止訪問，造成任何人都可以藉由網站，合法的將網站打包回家，其中可能還包含資料庫資料，使得惡意人士可以進階操作。

但若是使用過舊且含有弱點的系統或服務也是一種危害，透過已知弱點的方式，可以隨手抓取網路上的 PoC 來進行攻擊，所以服務需要定期升級上 Patch，不然你的作業系統也不會三不五時提醒你要更新了。

有時候是無意間造成的問題，可能出自於開發者疏忽或無知，像是透過 vim 來編輯檔案，通常會產生副檔名為 .swp 的暫存檔案，若是沒有清除，並且隨著服務上線，就會產生 Information Leak 的風險。（各種不同的編輯器都可能產生不同的暫存檔。）

### 重點整理

1.  是否使用過舊，或含有已知弱點風險的服務?
2.  是否有針對使用的框架、服務進行有效設定?
3.  Web App 的部分是否含有不安全的存取配置?
4.  是否妥善處理測試、暫存、備份檔?
5.  服務路徑下的權限是否設置得宜?

## Identity Management Testing

身分驗證管理，應該明確制定各種身分權限規則，什麼身分可以做什麼事情，基本權限如讀取、寫入、執行，在網頁服務上，執行可以歸類在可以使用哪些服務，以一個論壇服務為例子，系統管理員、網站管理員、一般使用者、訪客，不同的身分別所擁有的權限必定不一樣，若是一般使用者可以取得系統管理員的權限進行操作，便可以隨意刪除任意使用者的貼文，這是不被允許的。

此外帳戶的名稱也是此章節的一個重點，測試結構性的命名方式，以公司名稱加上流水號，如: mks01，或是常見的管理者名稱 admin、administrator、root 等…，都可能在測試的過程中獲得一些驚喜。

### 重點整理

1.  服務的權限是否清楚劃分?
2.  帳戶名稱、密碼命名是否存在結構性問題?（mks01,…,mks20/pwd01,…,pwd20）

## Authentication Testing

通常是網頁上的服務使用過簡單的身分驗證機制，讓 Session、Cookie 能夠被猜測，進而導致修改 Cookie 即可越權，並取得他人的身分，甚至可以拿到管理員權限，常見的配置方式都是直接拿帳號、名稱來當 Cookie 這種做法就相當不安全。

此外就是身分驗證的功能問題，一般較大型的網站，嘗試數次登入失敗之後，通常會跳出煩人的驗證碼，讓你登入還要做身分驗證，但也有遇過一些例子，只要將 URL 參數上的驗證碼 Key 與 Value 拿掉，就可以成功繞過判斷驗證碼的功能。

不論是網站還是手機、個人電腦，通常都慧要求密碼複雜度，不過這邊微軟就逆向操作了，在安裝 Windows 10 作業系統時，會要求使用者設定一組「建立超好記的密碼」，毫無密碼複雜度的密碼也是可以被接受的。

![Windows 建立超好記的密碼 ?](https://i.imgur.com/1khT4Dq.png)

至於為什麼會出現這種操作，我的觀點是自古以來，大多都不是因為使用者的帳號、密碼管理不當而造成資訊洩漏，過往我受邀回學校講資訊安全相關課程的時候，也曾經提到這一點，問題大多來自我們註冊的服務或網站，就拿 Garena 的知名線上遊戲 英雄聯盟來說（我也是受害者），官方遭害，造成眾多使用者帳戶遭到竊取，受害者究竟是企業還是玩家們呢？

![https://youtu.be/r2_J8STN7P4](https://i.imgur.com/pVBBnIo.png)

此外還有一種東西叫做「社工庫」，上面盡是各種服務網站資料庫洩漏之後的整合，如下圖可見還有概括「17直播」，這邊就不再贅述了，有興趣自行尋找社社工庫。

![](https://i.imgur.com/SLhYRd2.png)

講到這已經有點離題，但如果想知道自己的個資有沒有遭到洩漏，也可以使用以下兩個服務做驗證，只要 Email 就可以判別。

1.  [Hasso-Plattner-Institut](https://sec.hpi.de/ilc/search)
2.  [haveibeenpwned](https://haveibeenpwned.com/)

所以弱密碼到底可不可以使用？其實可以這樣想，假設你擁有管理權限的帳戶，而密碼不符合強度，惡意人士就不需要花費太多成本即可劫走你的帳戶，雖然難防有心人，但至少可以讓他人得手不是那麼容易。

### 重點整理

1.  是否全站使用 HTTPS 協定?
2.  是否變更使用的服務之預設密碼及關閉預設帳戶?
3.  是否有針對爆破行為進行管制?
4.  是否使用不安全的驗證身分機制?（cookie、session 有無加密?）
5.  是否允許弱密碼?（password、12345、123456）
6.  忘記密碼服務是否存在意外可能?（CRSF、修改其他使用者密碼）
7.  是否有配置 Cookie 有效時間?

## Authorization Testing

資安圈常在說「駭客思維」，總歸那就是下面這兩句話。

1.  駭客想的跟你不一樣
2.  永遠不要相信使用者

一般新手在寫程式的時候，總會為了達到功能要求，而忽略許多可能造成意外的環節，就好比說 URL 上存在 ?user=mksyi，那如果修改成 admin 會怎麼樣呢？又或者是檔案下載的功能實作含有問題，以 GET 請求並以參數 File 接著檔案名稱，在這樣的環節下，若惡意的竄改該參數的 Value 值，便構成任意下載漏洞的危害，藉此可以打包整個網站，甚至竊取系統上的敏感資料。

### 重點整理

1.  是否有不安全的權限控制機制?（透過修改參數就能越級?）
2.  是否限制服務的存取權限?（可能造成任意讀寫）
3.  是否針對使用者可控的部分進行限制?

## Session Management Testing

看完該章節的官方文件，認真覺得資安有一部分真的是需要通靈的，與 Authentication Testing 章節提到的 Cookie，概念有些類似，但這邊的測試方式是針對經過 Hash 的 Cookie，嘗試分析、猜測服務的原本 Hash 的內容為何，若是使用不安全的 Hash 規則來定義 Cookie，就會被利用來取得管理員權限。

此外也要清楚理解 Cookie/Session 的工作原理及作用域，包含一些安全相關的 HTTP Header [HttpOnly](https://www.owasp.org/index.php/HttpOnly)，甚至需要了解 [Same-Origin Policy (SOP)](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) 與 [Content Security Policy (CSP)](https://hackmd.io/@Eotones/BkOX6u5kX)。

除了猜測、爆破 Cookie 以外，大致上都是 XSS 的攻擊搭配社交工程來達到攻擊效果，通常不是竊取資料就是 CSRF。

### 重點整理

1.  是否使用過於簡單的規則產生 Cookie?
2.  是否配置相關安全的設定?（HttpOnly、SOP、CSP）

## Input Validation Testing

該章節基本上把使用者可輸入的地方隱含的弱點全部都提出來了，內容非常精彩，概括 OWASP TOP 10 上的所有漏洞之測試方法，文章寫到這邊，真的很猶豫是否要將每個漏洞都提出來講，還是日後再針對 OWASP TOP 10 撰寫一系列文章好?

思考了一下，還是提出幾個簡單常見的弱點出來說明，程式的流程簡化再簡化一般的程式流程，大致上可以分成以下三個部分。

![](https://i.imgur.com/6eCeZ8y.png)

而每個環節都有可能造成錯誤，Process 的部分可能不只包含一個服務在處理，比方說一個動態網頁包含前端的 HTML、後端語言 PHP、資料庫系統 MySQL 建構一個查詢服務，可能出錯的環節就包含 PHP、MySQL，甚至還要注意例外是由輸入，還是輸出所造成的，這部分比較常被忽視，就好比說儲存型的(Stored Cross-Site Scripting, S-XSS)，當使用者將惡意的 Script 語法當成輸入處理後，後端並沒有進行過濾，並且存入 MySQL 中，當讀出的時候瀏覽器將 Script 視為有意義的 HTML 並執行。

以上的例子乍看之下都是合理的，問題就在於太過信任使用者的輸入，而沒有進行過濾處理，而導致最終問題發生在 HTML 上，甚至還可以藉由輸入引發 SQL-Injection，進而對資料庫，甚至整個系統造成危害。

滲透測試人員針對不同的服務及功能，猜想可能造成的影響及危害，這邊又有個新詞彙「模糊測試（Fuzz Testing）」，是一種透過複雜的輸入，來針對服務功能進行測試是否出現例外的測試模式，就好比說一個僅能輸入整數的欄位，輸入字串是否可以被接受？其他編碼又會如何呢？又或者是輸入 CRLF 是否會被當成有意義的字元解析呢？

針對輸入的部分進行處理僅只是一層過濾，後續還需要思考到服務與服務間的傳輸問題，這邊介紹資安界大神 Orange 的文章 「[How I Chained 4 Bugs(Features?) into RCE on Amazon Collaboration System](https://blog.orange.tw/2018/08/how-i-chained-4-bugs-features-into-rce-on-amazon.html)」，其中有提到所謂 NginX 與 Tomcat 所造成的「不一致（inconsistency）」問題，有興趣的話可以自行翻閱。

### 重點整理

1.  開發者如何開發?
2.  後端如何處理輸入?
3.  哪個環節有可利用的可能?
4.  服務與服務之間是否存在不一致的問題?
5.  是否有 WAF 在作用?（過濾了些什麼? 如何 Bypass?）

## Testing for Error Handling

這部分的危害應該屬於 Information Leak，主要是尚未因應錯誤的頁面進行處理，造成頁面造成錯誤之後，可能出現服務的指紋（Fingerprint），另一種情況是開發者忘記關閉 Debug 模式，所以造成程式碼、程式路徑外洩，讓攻擊者可以獲得更多可利用資訊。

同時也需要針對不同的錯誤及例外進行處理，例如有一個登入的功能，使用者輸入帳號、密碼及驗證碼，錯誤不該統一為呈現「登入失敗」，這部分不屬於漏洞，但可能是作為開發者沒有個別處理輸入的標準吧?

### 重點整理

1.  設置對應的錯誤頁面，例如 404 Not Found Pages.
2.  關閉 Debug 模式
3.  針對不同得錯誤、例外進行個別處理。

## Testing for weak Cryptography

在台灣還有許多網站，甚至是購物網站，包含刷卡、線上購物功能的網站，都還沒有掛上「[超文本傳輸安全協定](https://zh.wikipedia.org/wiki/超文本传输安全协议)（HyperText Transfer Protocol Secure, HTTPS）」，這可能會造成「[中間人攻擊](https://zh.wikipedia.org/wiki/中间人攻击)（Man-in-the-middle attack, MITM）」，就由下圖來解釋吧。

![](https://i.imgur.com/bxBOOYU.png)

當資料以明文傳遞，資料只要可以被攔截，攻擊者就可以竄改或重送，若是遞送敏感資料，如帳號密碼、銀行卡號，後果就相當嚴重，所以傳輸過程需要「安全通訊協定」（Secure Sockets Layer, SSL）、「傳輸層安全性協定」（Transport Layer Security, TLS），確保資安三要素：機密性（Confidentiality）、完整性(Integrity） 、可用性（Availability）。

當掛上 HTTPS 就一定安全嗎？若服務使用已經被列為不安全加密演算法的加密方式，不論是數據本身的加密機制，還是傳輸過程的加密機制，就存在弱加密的風險，舉個幾個例子，像是 SSL 3.0 就已經被列入不安全的名單了，詳情可以閱讀「[Google 發現 SSL 3.0 漏洞，小心「貴賓犬」攻擊！](https://www.ithome.com.tw/news/91571)」與「[TLS加密協定竟然也不安全！企業須審慎內部漏洞](https://www.ithome.com.tw/promotion/93094)」。

### 重點整理

1.  是否沒有使用安全的傳輸協定?
2.  是否使用含有弱點的加密演算法?

## Business Logic Testing

關於邏輯漏洞比較常見的就是購物網站上的金額，最簡單的方式可以透過 F12，直接將 Value 上金額任意調整，甚至可以為負數，若是後端沒有驗證該項產品與金額，就會造成使用者以自訂的價格下單，這是非常嚴重的，資安圈有個很常上新聞的大大，多少應該都看過[張啟元](https://tw.appledaily.com/search/result?querystrS=張啟元)的新聞，就是運用這樣的手法。

該章節的官方文件提供非常完整的測試方法，在此我也是點到而已，通常涉及到資金、利益，開發者都會相當嚴謹的去處理各個環節，當然也少不了許多驗證，甚至任何請求都包含 Token 以防範 CSRF 的攻擊。

### 重點整理

1.  是否有些功能是沒有驗證的?（可以被繞過、竄改）
2.  是否有敏感的請求沒有夾帶 Token?（密碼變更、刷卡）
3.  請求失敗次數限制?
4.  是否處理非預期的請求、輸入?（是否經得起模糊測試的考驗?）

## Client Side Testing

用戶端的測試，不外乎就是 [XSS（Cross-site scripting）](https://zh.wikipedia.org/wiki/跨網站指令碼)，XSS 大致上可以分成三個類別。

1.  反射型
2.  儲存型
3.  DOM 型

自古以來許多人對 XSS 的定義就是彈個 Alert 窗，並沒有什麼危害，但其實這只是測試人員用來測試的一個方法而已，通常能跳窗，就代表惡意人士可以在站上任意撰寫、觸發 Script 腳本。

XSS 可以構成 CSRF 來造成更多危害，除了儲存型令人難以主動察覺以外，反射型與 DOM 型都可以從 URL 觀察出一些東西，但僅限於直接瀏覽，若是連結經過縮網址，進行兩層或以上的轉跳責另人難以察覺，甚至現在的瀏覽器也很聰明，會自動將 URL 遞送的參數做 URLencode，來過濾大部分 XSS 攻擊。

### 重點整理

1.  是否針對使用者輸入進行過濾?
2.  是否有針對可回顯得部分做 URLEncode、HTML 實體的處理?
3.  是否有 WAF?（WAF 如何過濾? 過濾了什麼?）

## 總結論

大部分的脆弱點、漏洞所造成的原因，都是一些缺乏換為思考所造成的，開發者單以自己角度撰寫程式，希望使用者同樣依照自己的邏輯操作，而忽視使用者可能使用預料之外的方式使用服務，而當沒有管控好意外，小則洩漏資訊，大則形成被利用的破口，就如同在 Input Validation Testing 所提及的，輸入、處理、輸出，問題可能存在於任何一個環節，同一個輸入在不同的服務上可能存在不同的認知或是定義，而這些都是開發者應該要注意的細節。

![](https://i.imgur.com/hxegzq3.jpg)

而從攻擊者的角度，分為黑箱跟白箱作業，大多數真實的環境還是黑箱為主，要如何在不知道原始碼的情況下，僅依靠輸入找出弱點，雖然 Web Application Security Testing 文件上有提供相當完整的測試方法，但真實環境更為複雜，更多時候是需要經驗扶持的，同時也是考驗測試者（攻擊者）的耐心，在不確定有無漏洞的情況下不斷嘗試，願意針對同一個問題執著，這邊引用翟本喬在 2015 SITCON 所講述的主題 [We hack worlds](https://www.youtube.com/watch?v=XbndvNrkFxs) 裡的一句話當結尾。

> 「  
> 　駭客是一種生活態度  
> 　一種 Life Style  
> 　一種凡是追根究柢的精神。  
> 　　　　　　　　　　　　　」 - 翟本喬
