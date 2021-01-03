---
title: "FastAPI 誕生的緣由"
tags: FastAPI 技術 網路 Python 轉載 翻譯
excerpt: "如果不是基於前人的成果，FastAPI 將不會存在。在 FastAPI 之前，前人已經創建了許多工具。在嘗試使用這些不同的框架，插件和工具來解決 FastAPI 涵蓋的所有功能的過程中發現，有的時候，沒有更好的辦法，除了……從以前的工具中汲取最佳創意，並以最佳方式將它們組合起來，使用以前甚至沒有的語言功能（Python 3.6+ 的類型提示），這就是 FastAPI。本文大約 5000 字。"
---

> 原文：https://fastapi.tiangolo.com/alternatives/
>
> 作者：tiangolo
>
> 翻譯：somenzz
>
> 本文講述了什麼啟發了 FastAPI 的誕生，它與其他替代框架的對比，以及從中汲取的經驗。
>

## 簡介

如果不是基於前人的成果，FastAPI 將不會存在。在 FastAPI 之前，前人已經創建了許多工具 。

幾年來，我一直在避免創建新框架。首先，我嘗試使用許多不同的框架，插件和工具來解決 **FastAPI** 涵蓋的所有功能。

但是有時候，沒有更好的辦法，除了創建具有所有這些功能的東西，從以前的工具中汲取最佳創意，並以最佳方式將它們組合起來，使用以前甚至沒有的語言功能（Python 3.6+ 的類型提示）。

## 在此之前的一些框架

### [Django](https://www.djangoproject.com/)

Django 是最流行的 Python 框架，受到廣泛信任。它可用於構建 Instagram 之類的大型系統。

它與關系數據庫（例如 MySQL 或 PostgreSQL）相對緊密地結合在一起，因此，以 NoSQL 數據庫（例如 Couchbase，MongoDB，Cassandra 等）作為 django 的主存儲引擎並不是一件容易的事。

創建它是為了在後端生成 HTML，而不是創建現代前端（例如 React，Vue.js 和Angular）或與其通信的其他系統（例如 IoT 設備）使用的 API 。

### [Django REST Framework](https://www.django-rest-framework.org/)

Django REST Framework 是一個非常靈活的框架，用於構建 Web API，以改善 Django 的 API 功能。

Mozilla，Red Hat 和 Eventbrite 等許多公司都使用它。

Django REST Framework 是第一個自動生成 API 文檔的框架，自動生成 API 的介面文檔是 FastAPI 框架誕生的緣由之一。

**註意**

Django REST Framework 框架的作者是 Tom Christie ，Tom Christie 也創造了 Starlette 和 Uvicorn。FastAPI 正是建立在 Starlette 和 Uvicorn 的基礎之上。

**啟發 FastAPI 地方**：有一個自動 API 文檔，Web 用戶界面可供用戶測試。

### [Flask](https://palletsprojects.com/p/flask/)

Flask 是一種輕量級的框架，它不包括數據庫集成，也沒有很多的附帶的功能，雖然這在 Django 那裡是默認提供的。

這個簡單性和靈活的特性允許使用 NoSQL 數據庫作為主數據存儲。盡管文檔在某些方面有所技術性，但它非常簡單，因此學習起來相對直觀。

它還常用於其他不需要數據庫，用戶管理或 Django 中預建功能的應用程式。盡管其中許多功能都可以通過添加插件來實現。

各個模塊之前的解耦，使之成為一個“微框架”，可以通過擴展為精確地提供所需的東西，這是我想要保留的一項關鍵功能。

考慮到 Flask 的簡單性，它似乎很適合構建 API。接下來要找到的是屬於 Flask的 “Django REST Framework”。

**啟發 FastAPI 地方：**成為一個微框架。易於混合和匹配所需的工具和零件。擁有一個簡單易用的路由系統。

### [Requests](https://requests.readthedocs.io/en/master/)

FastAPI 實際上不是 Requests 的替代工具。它們的適用範圍非常不同。實際上，在 FastAPI 應用程式內部使用 Requests 是很常見的。

但是，FastAPI 從 Requests 中獲得了很多啟發。Requests 是一個與API（作為客戶端）進行交互的庫，而 FastAPI 是一個用於構建 API（作為服務器）的庫。它們或多或少地處於相反的末端，彼此互補。Requests 具有非常簡單直觀的設計，非常易於使用，並具有合理的默認值。但同時，它非常強大且可自定義。

這就是為什麼，如官方網站所述：

> Requests 是有史以來下載次數最多的Python軟體包之一

您的使用方式非常簡單。例如，要發出GET請求，您可以編寫：

response = requests.get("http://example.com/some/url")

FastAPI 對應的 API 路徑操作如下所示：

```text
@app.get("/some/url")
def read_url():
  return {"message": "Hello World"}
```

它們使用起來的相似之處如 requests.get(...) 和 @app.get(...)。

**啟發 FastAPI 地方:**

- 擁有簡單直觀的API。
- 直接，直觀地使用HTTP方法名稱（操作）。
- 具有合理的默認值，功能強大的自定義。

### [Swagger](https://swagger.io/) / [OpenAPI](https://github.com/OAI/OpenAPI-Specification/)

我想要 Django REST Framework 的主要功能是自動 API 文檔。然後我發現 API 文檔有一個標準叫 Swagger ，它使用 JSON 或 YAML 來描述。

並且 Swagger API 的 Web 用戶界面已經被人創建出來了。因此，能夠為 API 生成Swagger 文檔將允許自動使用此 Web 用戶界面。

在某個時候，Swagger 被授予 Linux Foundation，將其重命名為 OpenAPI。這就是為什麼在談論版本 2.0 時通常會說“ Swagger”，對於版本3+來說是“ OpenAPI”。

**啟發 FastAPI 地方:**

為API規範採用開放標準，而不是使用自定義架構。並集成基於標準的用戶界面工具：

- [Swagger UI](https://github.com/swagger-api/swagger-ui)
- [ReDoc](https://github.com/Rebilly/ReDoc)

選擇這兩個是因為它們相當受歡迎且穩定，但是通過快速搜索，您可以找到數十個 OpenAPI 的其他替代用戶界面（可以與FastAPI一起使用）。

### Flask REST frameworks

有幾個 Flask REST frameworks ，但經過調查和試用，我發現，不少項目都停產或放棄，還存在有一些長期的問題，使得它們並不適合解決前面的問題。

### Marshmallow

一個由 API 系統所需的主要功能是數據的序列化，就是把數據從編程語言中的對象轉稱成可以在網路上傳輸的對象，比如數據庫中的數據轉換為 JSON 對象。將 Python 中的datetime 對象轉為字元串，等等。

另外一個功能就是數據的驗證，確保傳入的參數是有效的，例如，有些欄位是一個 int，類型而不是字元串，這在檢測輸入數據是非常有用的。

如果沒有數據驗證，你就必須用手工寫代碼來完成所有的檢查。

這兩點功能就是 Marshmallow 所提供的，這些是一個偉大的圖書館，之前我經常使用它。

Marshmallow 產生之前 Python 還沒有加入類型提示。因此，定義一個 schema 你需要引入 Marshmallow 特定的 utils 的和類。

**啟發 FastAPI 地方:**

使用代碼來定義提供的數據類型和驗證的 schema，驗證都是自動化的。

### [Webargs](https://webargs.readthedocs.io/en/latest/)

API 框架需要的另一大功能點是解析從前端發送的請求數據。Webargs (包括Flask) 是提供這一功能的工具，它採用 Marshmallow 做數據驗證。Webargs 和 Marshmallow 的作者是同一個開發人員。這是一個偉大的工具，在 FastAPI 誕生之前，我一直在用它。

**啟發 FastAPI 地方:**

對輸入的請求數據的自動驗證。

### [APISpec](https://apispec.readthedocs.io/en/stable/)

Marshmallow 和 Webargs 提供以插件形式提供驗證，解析和序列化。但文檔這塊缺失，然後 APISpec 誕生了。APISpec 可做為很多框架的插件（也是做為 Starlette 插件）。

它的工作方式是，在 Python 的文檔字元串內部使用 YAML 格式的描述來為每一個函數自動生成文檔。它會生成 OpenAPI 的 schemas。這也是它工作在 Flask, Starlette, Responder 等框架上的方式。

缺點是，我們又必須在 Python 的文檔字元串使用 YAML 語法，細微的差別可能導致一些錯誤。如果我們修改參數或 Marshmallow 的 schema，卻忘了還修改 YAML 文檔字元串，生成的模式將被廢棄。

APISpec 和 Marshmallow 的作者是同一個開發者。

**啟發 FastAPI 地方:**

支持API 的開放式標準。

### [Flask-apispec ](https://flask-apispec.readthedocs.io/en/latest/)

這是一個 Flask 插件，和 Webargs, Marshmallow, APISpec 聯系在一起。

APISpec 使用 Webargs 和 Marshmallow 生產的信息來生成 OpenAPI 的 schemas。 

這是一個偉大的工具，非常低估。它應該是比許多 Flask 插件更受歡迎。這可能是由於它的文檔過於簡潔、抽象。

它解決了無需在 Python文檔字元串內編寫YAML（另一種語法）。

在 FastAPI 創建之前，Flask, Flask-apispec， Marshmallow ，Webargs 的聯合是我經常用到的後端技術棧。使用這些框架，我們創建了幾個 Flask 的全棧生成器。以下是是我和幾個外部團隊一直到現在都使用的主要技術棧：

- [https://github.com/tiangolo/full-stack](https://github.com/tiangolo/full-stack)
- [https://github.com/tiangolo/full-stack-flask-couchbase](https://github.com/tiangolo/full-stack-flask-couchbase)
- [https://github.com/tiangolo/full-stack-flask-couchdb](https://github.com/tiangolo/full-stack-flask-couchdb)

**啟發 FastAPI 地方:**

自動生成的 OpenAPI 模式，使用相同的代碼定義序列化和驗證。

### [NestJS ](https://nestjs.com/)(and [Angular](https://angular.io/))

這很跟 Python 沒有關系，NestJS是一個JavaScript（TypeScript）NodeJS 框架，受Angular 啟發。它實現了一些功能，類似的，可以將它們用在 Flask-apispec 上。

它具有一個集成的依賴註入系統，同樣是受 Angular 啟發。像我知道的的其他依賴註入系統一樣，它需要預註冊，所以，它添加了冗長而重復的代碼。

由於參數由 TypeScript 類型（就像 Python 的類型提示一樣）描述，對編輯器的支持是相當不錯的。

TypeScript 的數據在編譯至 JavaScript 後並不保存，它不能依靠類型來實現驗證，序列化和文檔。由於這一點，一些設計決策，比如獲得的驗證，序列化和自動模式生成，它需要在很多地方加裝飾器。因此，它變得相當冗長。

對於嵌套模式它不能處理的非常好。因此，如果 JSON 體內又有 JSON 對象，這又是嵌套JSON對象JSON對象，它不能很好的生成文檔和驗證。

**啟發 FastAPI 地方**

使用 Python 類型提示可以提供很大的編輯器支持。

有一個強大的依賴註入系統。找到一種方法，以盡量減少重復的代碼。

### [Sanic](https://fastapi.tiangolo.com/alternatives/#sanic)

這是首批基於 asyncio 的極端快速 Python 框架之一。它和 Flask 非常相似。

它使用的 [uvloop](https://github.com/MagicStack/uvloop) 而不是 Python 默認的循環，因此非常快。它啟發了 Uvicorn 和 Starlette 的創建，後者在開放的基準方面比 Sanic 還要快。

**啟發 FastAPI 地方**

找到一個擁有極端性能表現的方法。

這就是為什麼 **FastAPI** 基於 Starlette，因為它是目前性能最高的框架（由第三方測試基準）。

### [Falcon](https://falconframework.org/)

Falcon 是另一個高性能的 Python 框架，它被設計成微型的做為其他框架的基礎，就像 Hug。

它使用以前的 WSGI 標準，這是一個同步框架，所以它不能處理像 WebSockets 和其他異步請求，不管怎麼說，它仍然有非常好的性能表現。

它被設計為具有接收兩個參數的函數，一個“請求”和一個“響應”。然後，您從請求中“讀取”部分，並將“部分”“寫入”響應。由於這種設計，不可能用標準Python類型提示將請求參數和主體聲明為函數參數。

因此，數據驗證，序列化和文檔編制必須以代碼而非自動完成。或者必須像 Hug 框架這樣將它們實現為 Falcon 之上。在受 Falcon 設計啟發的其他框架中，也是有一個請求對象和一個響應對象作為參數。

**啟發 FastAPI 地方**

尋找獲得出色性能的方法。

像 Hug（基於Falcon ） 一樣，FastAPI 在函數中聲明一個 response 參數。

在 FastAPI 這個是可選的，並且主要用於設置 Header，cookie 和備用狀態代碼。

### [Molten](https://moltenframework.com/)

我在構建 **FastAPI** 的最初階段發現了 Molten 。它們具有非常相似的想法：

- 基於Python類型提示。
- 基於這些類型提供驗證和生成文檔。
- 依賴註入系統。

它沒有使用像第三方庫（如Pydantic）提供數據驗證，序列化和文檔，它有自己的庫。因此，這些數據類型定義將不太容易重用。

它需要更多詳細的配置。並且由於它基於WSGI（而不是ASGI），因此其設計目的並不是要利用 Uvicorn，Starlette和Sanic 等工具提供的高性能能力。

依賴註入系統需要對依賴項進行預註冊，並且將基於已聲明的類型解決依賴問題。因此，不可能聲明多個組件來提供一個特定的類型。

路由在一個單獨的地方聲明，函數在另一個地方使用，（而不是在函數頂部使用裝飾器）。比起Flask（和Starlette）的實現方式，這更像 Django 的實現方式。它降低了代碼之間的耦合程度。

**啟發 FastAPI 地方**

使用模型欄位的默認值為數據類型定義額外的驗證，對編輯器支持更加友好，在 Pydantic 之前，這是不可行的。

這一點實際上也促進了 Pydantic 的部分模塊更新，以支持相同的驗證聲明樣式（所有這些功能現在在 Pydantic 中已經可用）。

### [Hug](https://www.hug.rest/)

Hug 是最早使用 Python 類型提示實現API參數類型聲明的框架之一。這是一個好主意，啟發了其他工具也這樣做。

它在聲明中使用了自定義類型，而不僅是 Python 的標準類型，但這仍然是巨大的進步。

它也是第一個生成自定義模式的框架，該自定義模式以 JSON 聲明整個 API。

它不是基於 OpenAPI 和 JSON Schema 之類的標準。因此，將其與 Swagger UI 等其他工具集成並不是一件容易的事。但這又是一個非常創新的想法。

它具有一個有趣而罕見的功能：使用相同的框架，可以創建 API 以及 CLI。

由於它基於先前的 Python 同步 Web 框架（WSGI）標準，盡管它仍然具有高性能，但它不能處理 Websockets 和其他事物。

**啟發 FastAPI 地方**

Hug 啟發了 APIStar 的各個部分，Hug 與 APIStar 是我發現最有前途的工具之一。

Hug 啟發了 **FastAPI** 使用 Python 類型提示來聲明參數，並自動生成定義 API 的模式。

Hug 啟發了 **FastAPI** 在函數中聲明一個 response 參數在用於設置標頭和 cookie。

### [APIStar](https://github.com/encode/apistar)（<= 0.5）

在決定構建 **FastAPI** 之前，我發現了**APIStar** 服務器。它幾乎滿足了我的所有需求，並且設計出色。

這是最早使用Python類型提示聲明參數和請求的框架之一（在NestJS和Molten之前）。我在發現 Hub 框架的同時也發現了它。但是 APIStar 使用了OpenAPI 標準。

基於相同的類型提示，它擁有自動化的數據驗證，數據序列化和 生成 OpenAPI 的模式。

主體模式的定義沒有使用 Python 的類型提示，它與 Marshmallow 有點相似，因此，對編輯器的支持不會那麼好，但是 APIStar 仍然是最好的選擇。它具有最佳性能基準（僅被 Starlette 超越）。

最初，它沒有自動化 API 文檔的 Web UI，但我知道我可以向其中添加 Swagger UI。它有一個依賴註入系統。與上面討論的其他工具一樣，它需要組件的預註冊。但是，這仍然是一個很棒的功能。

我從未在完整的項目中使用過它，因為它沒有安全性集成，因此，我無法用基於 Flask-apispec 的全棧生成器替換我擁有的所有功能。我在項目積壓中創建了添加該功能的請求。

但是隨後，該項目的重點轉移了。它不再是一個API Web 框架，因為創建者需要專註於Starlette。現在，APIStar 是一組用於驗證 OpenAPI 規範的工具，而不是 Web框架。

APIStar 是由 Tom Christie 創建的，他也創建了以：

- Django REST框架
- Starlette（FastAPI所基於的）
- Uvicorn（由 Starlette 和 FastAPI 使用）

**啟發 FastAPI 地方**

我認為用相同的 Python 類型聲明多個內容（數據驗證，序列化和文檔），同時又提供了強大的編輯器支持，這是非常絕妙的主意。

在長時間尋找相似的框架並測試了許多不同的替代方案之後，APIStar 是最佳的選擇。然後，APIStar 不再作為服務器存在，然後 Starlette 出現了，並且為此類系統提供了新的更好的基礎。那是構建**FastAPI**的最終靈感。

我認為，FastAPI 是 APIStar 的“精神上的繼任者”，同時基於對所有這些先前工具的學習，在改進和增加功能，鍵入系統和其他部分的同時，也是如此。

## **FastAPI 使用的框架**

### [Pydantic](https://pydantic-docs.helpmanual.io/)

Pydantic 是一個庫，基於Python類型提示來定義數據驗證，序列化和文檔（使用JSON模式）。這使其非常直觀。它可與 Marshmallow 媲美。盡管在基準測試中它比Marshmallow 更快。並且由於它基於相同的Python類型提示，因此對編輯器的支持非常棒。

**FastAPI使用它來**處理所有數據驗證，數據序列化和自動模型文檔（基於JSON Schema）。

然後，**FastAPI** 會獲取該 JSON Schema 數據並將其放入OpenAPI 中，除此之外它還會執行其他所有操作。

### [Starlette](https://www.starlette.io/)

Starlette 是一種輕量級的 ASGI 框架/工具包，是構建高性能 asyncio 服務的理想選擇。

它非常簡單直觀。它的設計易於擴展，並具有模塊化組件。

它具有：

- 令人印象深刻的性能。
- WebSocket支持。
- GraphQL支持。
- 處理中的後台任務。
- 啟動和關閉事件。
- 測試基於 requests 的客戶端。
- CORS，GZip，靜態文件，流式響應。
- 會話和 Cookie 支持。
- 100％ 的測試覆蓋率。
- 100％ 類型註釋的代碼庫。
- 零硬依賴性。

Starlette 是目前測試最快的 Python 框架。只有 Uvicorn 超越了它，Uvicorn 不是框架，而是服務器。

Starlette 提供了所有基本的 Web 微框架功能。但是它不提供自動數據驗證，序列化或API 文檔。

這是 **FastAPI** 在頂部添加的主要內容之一，全部基於Python類型提示（使用Pydantic）。以及依賴註入系統，安全實用程式，OpenAPI 模式生成等。

**技術細節：** ASGI 是 Django 核心團隊成員開發的新“標準”。盡管他們正在這樣做，但它仍然不是“ Python標準”（PEP）。但是，它已經被多種工具用作“標準”。這可以大大提高互操作性，因為您可以將 Uvicorn 切換到任何其他 ASGI 服務器（例如 Daphne 或 Hypercorn），也可以添加與ASGI相容的工具，例如 python-socketio。

FastAPI 使用它來處理所有核心 Web 部件。在頂部添加功能。類 FastAPI 本身直接繼承Starlette。因此，使用 Starlette 可以執行的任何操作，都可以直接使用 FastAPI 進行**。**

### [Uvicorn](https://www.uvicorn.org/)

Uvicorn 是基於 uvloop 和 httptools 構建的如閃電般快速的 ASGI 服務器。它不是Web框架，而是服務器。例如，它不提供用於按路徑進行路由的工具。那是像 Starlette（或FastAPI）這樣的框架可以提供的。它是 Starlette 和 FastAPI 的推薦服務器。

FastAPI 推薦它為主 Web服務器運行 FastAPI 應用程式。您可以將其與 Gunicorn 結合使用，以擁有異步多進程服務器。在“ [部署”](https://fastapi.tiangolo.com/deployment/) 部分中查看更多詳細信息。
