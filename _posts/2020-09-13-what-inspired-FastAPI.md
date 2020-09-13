---
title: "FastAPI 诞生的缘由"
tags: FastAPI 技术 网络 Python 转载 翻译
---

如果不是基于前人的成果，FastAPI 将不会存在。在 FastAPI 之前，前人已经创建了许多工具。
在尝试使用这些不同的框架，插件和工具来解决 FastAPI 涵盖的所有功能的过程中发现，有的时候，没有更好的办法，除了……
从以前的工具中汲取最佳创意，并以最佳方式将它们组合起来，使用以前甚至没有的语言功能（Python 3.6+ 的类型提示），这就是 FastAPI：

- 有一个自动 API 文档，Web 用户界面可供用户测试。

- 拥有一个简单易用的路由系统。

- 为API规范采用开放标准，而不是使用自定义架构。

- 自动生成的 OpenAPI 模式，并集成了基于标准的用户界面工具。

- 使用代码来定义提供的数据类型和验证的 schema，验证都是自动化的。

- 使用 Python 类型提示。

- 有一个强大的依赖注入系统。找到一种方法，以尽量减少重复的代码。

- 基于 Starlette，因为它是目前性能最高的框架（由第三方测试基准）。

<!--more-->

原文：https://fastapi.tiangolo.com/alternatives/

作者：tiangolo

翻译：somenzz

本文讲述了什么启发了 FastAPI 的诞生，它与其他替代框架的对比，以及从中汲取的经验。

## 简介

如果不是基于前人的成果，FastAPI 将不会存在。在 FastAPI 之前，前人已经创建了许多工具 。

几年来，我一直在避免创建新框架。首先，我尝试使用许多不同的框架，插件和工具来解决 **FastAPI** 涵盖的所有功能。

但是有时候，没有更好的办法，除了创建具有所有这些功能的东西，从以前的工具中汲取最佳创意，并以最佳方式将它们组合起来，使用以前甚至没有的语言功能（Python 3.6+ 的类型提示）。

## 在此之前的一些框架

### [Django](https://www.djangoproject.com/)

Django 是最流行的 Python 框架，受到广泛信任。它可用于构建 Instagram 之类的大型系统。

它与关系数据库（例如 MySQL 或 PostgreSQL）相对紧密地结合在一起，因此，以 NoSQL 数据库（例如 Couchbase，MongoDB，Cassandra 等）作为 django 的主存储引擎并不是一件容易的事。

创建它是为了在后端生成 HTML，而不是创建现代前端（例如 React，Vue.js 和Angular）或与其通信的其他系统（例如 IoT 设备）使用的 API 。

### [Django REST Framework](https://www.django-rest-framework.org/)

Django REST Framework 是一个非常灵活的框架，用于构建 Web API，以改善 Django 的 API 功能。

Mozilla，Red Hat 和 Eventbrite 等许多公司都使用它。

Django REST Framework 是第一个自动生成 API 文档的框架，自动生成 API 的接口文档是 FastAPI 框架诞生的缘由之一。

**注意**

Django REST Framework 框架的作者是 Tom Christie ，Tom Christie 也创造了 Starlette 和 Uvicorn。FastAPI 正是建立在 Starlette 和 Uvicorn 的基础之上。

**启发 FastAPI 地方**：有一个自动 API 文档，Web 用户界面可供用户测试。

### [Flask](https://palletsprojects.com/p/flask/)

Flask 是一种轻量级的框架，它不包括数据库集成，也没有很多的附带的功能，虽然这在 Django 那里是默认提供的。

这个简单性和灵活的特性允许使用 NoSQL 数据库作为主数据存储。尽管文档在某些方面有所技术性，但它非常简单，因此学习起来相对直观。

它还常用于其他不需要数据库，用户管理或 Django 中预建功能的应用程序。尽管其中许多功能都可以通过添加插件来实现。

各个模块之前的解耦，使之成为一个“微框架”，可以通过扩展为精确地提供所需的东西，这是我想要保留的一项关键功能。

考虑到 Flask 的简单性，它似乎很适合构建 API。接下来要找到的是属于 Flask的 “Django REST Framework”。

**启发 FastAPI 地方：**成为一个微框架。易于混合和匹配所需的工具和零件。拥有一个简单易用的路由系统。

### [Requests](https://requests.readthedocs.io/en/master/)

FastAPI 实际上不是 Requests 的替代工具。它们的适用范围非常不同。实际上，在 FastAPI 应用程序内部使用 Requests 是很常见的。

但是，FastAPI 从 Requests 中获得了很多启发。Requests 是一个与API（作为客户端）进行交互的库，而 FastAPI 是一个用于构建 API（作为服务器）的库。它们或多或少地处于相反的末端，彼此互补。Requests 具有非常简单直观的设计，非常易于使用，并具有合理的默认值。但同时，它非常强大且可自定义。

这就是为什么，如官方网站所述：

> Requests 是有史以来下载次数最多的Python软件包之一

您的使用方式非常简单。例如，要发出GET请求，您可以编写：

response = requests.get("http://example.com/some/url")

FastAPI 对应的 API 路径操作如下所示：

```text
@app.get("/some/url")
def read_url():
  return {"message": "Hello World"}
```

它们使用起来的相似之处如 requests.get(...) 和 @app.get(...)。

**启发 FastAPI 地方:**

- 拥有简单直观的API。
- 直接，直观地使用HTTP方法名称（操作）。
- 具有合理的默认值，功能强大的自定义。

### [Swagger](https://swagger.io/) / [OpenAPI](https://github.com/OAI/OpenAPI-Specification/)

我想要 Django REST Framework 的主要功能是自动 API 文档。然后我发现 API 文档有一个标准叫 Swagger ，它使用 JSON 或 YAML 来描述。

并且 Swagger API 的 Web 用户界面已经被人创建出来了。因此，能够为 API 生成Swagger 文档将允许自动使用此 Web 用户界面。

在某个时候，Swagger 被授予 Linux Foundation，将其重命名为 OpenAPI。这就是为什么在谈论版本 2.0 时通常会说“ Swagger”，对于版本3+来说是“ OpenAPI”。

**启发 FastAPI 地方:**

为API规范采用开放标准，而不是使用自定义架构。并集成基于标准的用户界面工具：

- [Swagger UI](https://github.com/swagger-api/swagger-ui)
- [ReDoc](https://github.com/Rebilly/ReDoc)

选择这两个是因为它们相当受欢迎且稳定，但是通过快速搜索，您可以找到数十个 OpenAPI 的其他替代用户界面（可以与FastAPI一起使用）。

### Flask REST frameworks

有几个 Flask REST frameworks ，但经过调查和试用，我发现，不少项目都停产或放弃，还存在有一些长期的问题，使得它们并不适合解决前面的问题。

### Marshmallow

一个由 API 系统所需的主要功能是数据的序列化，就是把数据从编程语言中的对象转称成可以在网络上传输的对象，比如数据库中的数据转换为 JSON 对象。将 Python 中的datetime 对象转为字符串，等等。

另外一个功能就是数据的验证，确保传入的参数是有效的，例如，有些字段是一个 int，类型而不是字符串，这在检测输入数据是非常有用的。

如果没有数据验证，你就必须用手工写代码来完成所有的检查。

这两点功能就是 Marshmallow 所提供的，这些是一个伟大的图书馆，之前我经常使用它。

Marshmallow 产生之前 Python 还没有加入类型提示。因此，定义一个 schema 你需要引入 Marshmallow 特定的 utils 的和类。

**启发 FastAPI 地方:**

使用代码来定义提供的数据类型和验证的 schema，验证都是自动化的。

### [Webargs](https://webargs.readthedocs.io/en/latest/)

API 框架需要的另一大功能点是解析从前端发送的请求数据。Webargs (包括Flask) 是提供这一功能的工具，它采用 Marshmallow 做数据验证。Webargs 和 Marshmallow 的作者是同一个开发人员。这是一个伟大的工具，在 FastAPI 诞生之前，我一直在用它。

**启发 FastAPI 地方:**

对输入的请求数据的自动验证。

### [APISpec](https://apispec.readthedocs.io/en/stable/)

Marshmallow 和 Webargs 提供以插件形式提供验证，解析和序列化。但文档这块缺失，然后 APISpec 诞生了。APISpec 可做为很多框架的插件（也是做为 Starlette 插件）。

它的工作方式是，在 Python 的文档字符串内部使用 YAML 格式的描述来为每一个函数自动生成文档。它会生成 OpenAPI 的 schemas。这也是它工作在 Flask, Starlette, Responder 等框架上的方式。

缺点是，我们又必须在 Python 的文档字符串使用 YAML 语法，细微的差别可能导致一些错误。如果我们修改参数或 Marshmallow 的 schema，却忘了还修改 YAML 文档字符串，生成的模式将被废弃。

APISpec 和 Marshmallow 的作者是同一个开发者。

**启发 FastAPI 地方:**

支持API 的开放式标准。

### [Flask-apispec ](https://flask-apispec.readthedocs.io/en/latest/)

这是一个 Flask 插件，和 Webargs, Marshmallow, APISpec 联系在一起。

APISpec 使用 Webargs 和 Marshmallow 生产的信息来生成 OpenAPI 的 schemas。 

这是一个伟大的工具，非常低估。它应该是比许多 Flask 插件更受欢迎。这可能是由于它的文档过于简洁、抽象。

它解决了无需在 Python文档字符串内编写YAML（另一种语法）。

在 FastAPI 创建之前，Flask, Flask-apispec， Marshmallow ，Webargs 的联合是我经常用到的后端技术栈。使用这些框架，我们创建了几个 Flask 的全栈生成器。以下是是我和几个外部团队一直到现在都使用的主要技术栈：

- [https://github.com/tiangolo/full-stack](https://github.com/tiangolo/full-stack)
- [https://github.com/tiangolo/full-stack-flask-couchbase](https://github.com/tiangolo/full-stack-flask-couchbase)
- [https://github.com/tiangolo/full-stack-flask-couchdb](https://github.com/tiangolo/full-stack-flask-couchdb)

**启发 FastAPI 地方:**

自动生成的 OpenAPI 模式，使用相同的代码定义序列化和验证。

### [NestJS ](https://nestjs.com/)(and [Angular](https://angular.io/))

这很跟 Python 没有关系，NestJS是一个JavaScript（TypeScript）NodeJS 框架，受Angular 启发。它实现了一些功能，类似的，可以将它们用在 Flask-apispec 上。

它具有一个集成的依赖注入系统，同样是受 Angular 启发。像我知道的的其他依赖注入系统一样，它需要预注册，所以，它添加了冗长而重复的代码。

由于参数由 TypeScript 类型（就像 Python 的类型提示一样）描述，对编辑器的支持是相当不错的。

TypeScript 的数据在编译至 JavaScript 后并不保存，它不能依靠类型来实现验证，序列化和文档。由于这一点，一些设计决策，比如获得的验证，序列化和自动模式生成，它需要在很多地方加装饰器。因此，它变得相当冗长。

对于嵌套模式它不能处理的非常好。因此，如果 JSON 体内又有 JSON 对象，这又是嵌套JSON对象JSON对象，它不能很好的生成文档和验证。

**启发 FastAPI 地方**

使用 Python 类型提示可以提供很大的编辑器支持。

有一个强大的依赖注入系统。找到一种方法，以尽量减少重复的代码。

### [Sanic](https://fastapi.tiangolo.com/alternatives/#sanic)

这是首批基于 asyncio 的极端快速 Python 框架之一。它和 Flask 非常相似。

它使用的 [uvloop](https://github.com/MagicStack/uvloop) 而不是 Python 默认的循环，因此非常快。它启发了 Uvicorn 和 Starlette 的创建，后者在开放的基准方面比 Sanic 还要快。

**启发 FastAPI 地方**

找到一个拥有极端性能表现的方法。

这就是为什么 **FastAPI** 基于 Starlette，因为它是目前性能最高的框架（由第三方测试基准）。

### [Falcon](https://falconframework.org/)

Falcon 是另一个高性能的 Python 框架，它被设计成微型的做为其他框架的基础，就像 Hug。

它使用以前的 WSGI 标准，这是一个同步框架，所以它不能处理像 WebSockets 和其他异步请求，不管怎么说，它仍然有非常好的性能表现。

它被设计为具有接收两个参数的函数，一个“请求”和一个“响应”。然后，您从请求中“读取”部分，并将“部分”“写入”响应。由于这种设计，不可能用标准Python类型提示将请求参数和主体声明为函数参数。

因此，数据验证，序列化和文档编制必须以代码而非自动完成。或者必须像 Hug 框架这样将它们实现为 Falcon 之上。在受 Falcon 设计启发的其他框架中，也是有一个请求对象和一个响应对象作为参数。

**启发 FastAPI 地方**

寻找获得出色性能的方法。

像 Hug（基于Falcon ） 一样，FastAPI 在函数中声明一个 response 参数。

在 FastAPI 这个是可选的，并且主要用于设置 Header，cookie 和备用状态代码。

### [Molten](https://moltenframework.com/)

我在构建 **FastAPI** 的最初阶段发现了 Molten 。它们具有非常相似的想法：

- 基于Python类型提示。
- 基于这些类型提供验证和生成文档。
- 依赖注入系统。

它没有使用像第三方库（如Pydantic）提供数据验证，序列化和文档，它有自己的库。因此，这些数据类型定义将不太容易重用。

它需要更多详细的配置。并且由于它基于WSGI（而不是ASGI），因此其设计目的并不是要利用 Uvicorn，Starlette和Sanic 等工具提供的高性能能力。

依赖注入系统需要对依赖项进行预注册，并且将基于已声明的类型解决依赖问题。因此，不可能声明多个组件来提供一个特定的类型。

路由在一个单独的地方声明，函数在另一个地方使用，（而不是在函数顶部使用装饰器）。比起Flask（和Starlette）的实现方式，这更像 Django 的实现方式。它降低了代码之间的耦合程度。

**启发 FastAPI 地方**

使用模型字段的默认值为数据类型定义额外的验证，对编辑器支持更加友好，在 Pydantic 之前，这是不可行的。

这一点实际上也促进了 Pydantic 的部分模块更新，以支持相同的验证声明样式（所有这些功能现在在 Pydantic 中已经可用）。

### [Hug](https://www.hug.rest/)

Hug 是最早使用 Python 类型提示实现API参数类型声明的框架之一。这是一个好主意，启发了其他工具也这样做。

它在声明中使用了自定义类型，而不仅是 Python 的标准类型，但这仍然是巨大的进步。

它也是第一个生成自定义模式的框架，该自定义模式以 JSON 声明整个 API。

它不是基于 OpenAPI 和 JSON Schema 之类的标准。因此，将其与 Swagger UI 等其他工具集成并不是一件容易的事。但这又是一个非常创新的想法。

它具有一个有趣而罕见的功能：使用相同的框架，可以创建 API 以及 CLI。

由于它基于先前的 Python 同步 Web 框架（WSGI）标准，尽管它仍然具有高性能，但它不能处理 Websockets 和其他事物。

**启发 FastAPI 地方**

Hug 启发了 APIStar 的各个部分，Hug 与 APIStar 是我发现最有前途的工具之一。

Hug 启发了 **FastAPI** 使用 Python 类型提示来声明参数，并自动生成定义 API 的模式。

Hug 启发了 **FastAPI** 在函数中声明一个 response 参数在用于设置标头和 cookie。

### [APIStar](https://github.com/encode/apistar)（<= 0.5）

在决定构建 **FastAPI** 之前，我发现了**APIStar** 服务器。它几乎满足了我的所有需求，并且设计出色。

这是最早使用Python类型提示声明参数和请求的框架之一（在NestJS和Molten之前）。我在发现 Hub 框架的同时也发现了它。但是 APIStar 使用了OpenAPI 标准。

基于相同的类型提示，它拥有自动化的数据验证，数据序列化和 生成 OpenAPI 的模式。

主体模式的定义没有使用 Python 的类型提示，它与 Marshmallow 有点相似，因此，对编辑器的支持不会那么好，但是 APIStar 仍然是最好的选择。它具有最佳性能基准（仅被 Starlette 超越）。

最初，它没有自动化 API 文档的 Web UI，但我知道我可以向其中添加 Swagger UI。它有一个依赖注入系统。与上面讨论的其他工具一样，它需要组件的预注册。但是，这仍然是一个很棒的功能。

我从未在完整的项目中使用过它，因为它没有安全性集成，因此，我无法用基于 Flask-apispec 的全栈生成器替换我拥有的所有功能。我在项目积压中创建了添加该功能的请求。

但是随后，该项目的重点转移了。它不再是一个API Web 框架，因为创建者需要专注于Starlette。现在，APIStar 是一组用于验证 OpenAPI 规范的工具，而不是 Web框架。

APIStar 是由 Tom Christie 创建的，他也创建了以：

- Django REST框架
- Starlette（FastAPI所基于的）
- Uvicorn（由 Starlette 和 FastAPI 使用）

**启发 FastAPI 地方**

我认为用相同的 Python 类型声明多个内容（数据验证，序列化和文档），同时又提供了强大的编辑器支持，这是非常绝妙的主意。

在长时间寻找相似的框架并测试了许多不同的替代方案之后，APIStar 是最佳的选择。然后，APIStar 不再作为服务器存在，然后 Starlette 出现了，并且为此类系统提供了新的更好的基础。那是构建**FastAPI**的最终灵感。

我认为，FastAPI 是 APIStar 的“精神上的继任者”，同时基于对所有这些先前工具的学习，在改进和增加功能，键入系统和其他部分的同时，也是如此。

## **FastAPI 使用的框架**

### [Pydantic](https://pydantic-docs.helpmanual.io/)

Pydantic 是一个库，基于Python类型提示来定义数据验证，序列化和文档（使用JSON模式）。这使其非常直观。它可与 Marshmallow 媲美。尽管在基准测试中它比Marshmallow 更快。并且由于它基于相同的Python类型提示，因此对编辑器的支持非常棒。

**FastAPI使用它来**处理所有数据验证，数据序列化和自动模型文档（基于JSON Schema）。

然后，**FastAPI** 会获取该 JSON Schema 数据并将其放入OpenAPI 中，除此之外它还会执行其他所有操作。

### [Starlette](https://www.starlette.io/)

Starlette 是一种轻量级的 ASGI 框架/工具包，是构建高性能 asyncio 服务的理想选择。

它非常简单直观。它的设计易于扩展，并具有模块化组件。

它具有：

- 令人印象深刻的性能。
- WebSocket支持。
- GraphQL支持。
- 处理中的后台任务。
- 启动和关闭事件。
- 测试基于 requests 的客户端。
- CORS，GZip，静态文件，流式响应。
- 会话和 Cookie 支持。
- 100％ 的测试覆盖率。
- 100％ 类型注释的代码库。
- 零硬依赖性。

Starlette 是目前测试最快的 Python 框架。只有 Uvicorn 超越了它，Uvicorn 不是框架，而是服务器。

Starlette 提供了所有基本的 Web 微框架功能。但是它不提供自动数据验证，序列化或API 文档。

这是 **FastAPI** 在顶部添加的主要内容之一，全部基于Python类型提示（使用Pydantic）。以及依赖注入系统，安全实用程序，OpenAPI 模式生成等。

**技术细节：** ASGI 是 Django 核心团队成员开发的新“标准”。尽管他们正在这样做，但它仍然不是“ Python标准”（PEP）。但是，它已经被多种工具用作“标准”。这可以大大提高互操作性，因为您可以将 Uvicorn 切换到任何其他 ASGI 服务器（例如 Daphne 或 Hypercorn），也可以添加与ASGI兼容的工具，例如 python-socketio。

FastAPI 使用它来处理所有核心 Web 部件。在顶部添加功能。类 FastAPI 本身直接继承Starlette。因此，使用 Starlette 可以执行的任何操作，都可以直接使用 FastAPI 进行**。**

### [Uvicorn](https://www.uvicorn.org/)

Uvicorn 是基于 uvloop 和 httptools 构建的如闪电般快速的 ASGI 服务器。它不是Web框架，而是服务器。例如，它不提供用于按路径进行路由的工具。那是像 Starlette（或FastAPI）这样的框架可以提供的。它是 Starlette 和 FastAPI 的推荐服务器。

FastAPI 推荐它为主 Web服务器运行 FastAPI 应用程序。您可以将其与 Gunicorn 结合使用，以拥有异步多进程服务器。在“ [部署”](https://fastapi.tiangolo.com/deployment/) 部分中查看更多详细信息。
