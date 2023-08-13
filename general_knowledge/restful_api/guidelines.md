# RESTful API 编写指南

基于一些不错的RESTful开发组件，可以快速的开发出不错的RESTful API，但如果不了解开发规范的、健壮的RESTful
API的基本面，即便优秀的RESTful开发组件摆在面前，也无法很好的理解和使用。

下文Gevin结合自己的实践经验，整理了从零开始开发RESTful API的核心要点，完善的RESTful开发组件基本都会包含全部或大部分要点，对于支持不够到位的要点，我们也可以自己写代码实现。

## Outline

- Request 和 Response
- Serialization 和 Deserialization
- Validation
- Authentication 和 Permission
- CORS
- URL Rules

## 1. Request 和 Response

RESTful API的开发和使用，无非是客户端向服务器发请求（request），以及服务器对客户端请求的响应（response）。本真RESTful架构风格具有统一接口的特点，即，使用不同的http方法表达不同的行为：

- GET（SELECT）：从服务器取出资源（一项或多项）
- POST（CREATE）：在服务器新建一个资源
- PUT（UPDATE）：在服务器更新资源（客户端提供完整资源数据）
- PATCH（UPDATE）：在服务器更新资源（客户端提供需要修改的资源数据）
- DELETE（DELETE）：从服务器删除资源

客户端会基于`GET`方法向服务器发送获取数据的请求，基于`PUT`或`PATCH`
方法向服务器发送更新数据的请求等，服务端在设计API时，也要按照相应规范来处理对应的请求，这点现在应该已经成为所有RESTful
API的开发者的共识了，而且各web框架的request类和response类都很强大，具有合理的默认设置和灵活的定制性，Gevin在这里仅准备强调一下响应这些request时，常用的Response要包含的数据和状态码（status
code），不完善的内容，欢迎大家补充:

- 当 `GET`, `PUT`和`PATCH`请求成功时，要返回对应的数据，及状态码`200`，即`SUCCESS`
- 当`POST`创建数据成功时，要返回创建的数据，及状态码`201`，即`CREATED`
- 当`DELETE`删除数据成功时，不返回数据，状态码要返回`204`，即`NO CONTENT`
- 当`GET`不到数据时，状态码要返回`404`，即`NOT FOUND`
- 任何时候，如果请求有问题，如校验请求数据时发现错误，要返回状态码`400`，即`BAD REQUEST`
- 当API 请求需要用户认证时，如果request中的认证信息不正确，要返回状态码`401`，即`NOT AUTHORIZED`
- 当API 请求需要验证用户权限时，如果当前用户无相应权限，要返回状态码`403`，即`FORBIDDEN`

最后，关于 `Request`和`Response`，不要忽略了`HEADER`中的`Content-Type`。以`json`
为例，如果API要求客户端发送request时要传入`json`数据，则服务器端仅做好`json`
数据的获取和解析即可，但如果服务端支持多种类型数据的传入，如同时支持`json`和`form-data`，则要根据客户端发送请求时`header`
中的`Content-Type`，对不同类型是数据分别实现获取和解析；如果API响应客户端请求后，需要返回`json`数据，需要在`header`
中添加`Content-Type=application/json`。

## 2. Serialization 和 Deserialization

Serialization 和 Deserialization即序列化和反序列化。

RESTful API以规范统一的格式作为数据的载体，常用的格式为`json`或`xml`
，以json格式为例，当客户端向服务器发请求时，或者服务器相应客户端的请求，向客户端返回数据时，都是传输json格式的文本，而在服务器内部，数据处理时基本不用json格式的字符串，而是native类型的数据，最典型的如类的实例，即对象（object），json仅为服务器和客户端通信时，在网络上传输的数据的格式，服务器和客户端内部，均存在将json转为native类型数据和将native类型数据转为json的需求，其中，将native类型数据转为json即为`序列化`
，将json转为native类型数据即为`反序列化`。虽然某些开发语言，如`Python`
，其原生数据类型list和dict能轻易实现序列化和反序列化，但对于复杂的API，内部实现时总会以对象作为数据的载体，因此，确保序列化和反序列化方法的实现，是开发RESTful
API最重要的一步准备工作

> 题外话，序列化和反序列化的便捷，造就了RESTful API跨平台的特点，使得REST取代RPC成为Web Service的主流

序列化和反序列化是RESTful API开发中的一项硬需求，所以几乎每一种常用的开发语言都会有一个或多个优秀的开源库，来实现序列化和反序列化，因此，我们在开发RESTful
API时，没必要制造重复的轮子，选一个好用的库即可，如`Python`中的`marshmallow`。

## 3. Validation

Validation即数据校验，是开发健壮RESTful API中另一个重要的一环。仍以json为例，当客户端向服务器发出`post`, `put`或`patch`
请求时，通常会同时给服务器发送json格式的相关数据，服务器在做数据处理之前，先做数据校验，是最合理和安全的前后端交互。如果客户端发送的数据不正确或不合理，服务器端经过校验后直接向客户端返回400错误及相应的数据错误信息即可。

常见的数据校验包括：

- 数据类型校验，如字段类型如果是int，那么给字段赋字符串的值则报错
- 数据格式校验，如邮箱或密码，其赋值必须满足相应的正则表达式，才是正确的输入数据
- 数据逻辑校验，如数据包含出生日期和年龄两个字段，如果这两个字段的数据不一致，则数据校验失败

以上三种类型的校验，数据逻辑校验最为复杂，通常涉及到多个字段的配合，或者要结合用户和权限做相应的校验。Validation虽然是RESTful
API 编写中的一个可选项，但它对API的安全、服务器的开销和交互的友好性而言，都具有重要意义，因此，Gevin建议，开发一套完善的RESTful
API时，Validation的实现必不可少。

## 4. Authentication 和 Permission

Authentication指用户认证，Permission指权限机制，这两点是使RESTful API 强大、灵活和安全的基本保障。

常用的认证机制是`Basic Auth`和`OAuth`，RESTful API 开发中，除非API非常简单，且没有潜在的安全性问题，否则，认证机制是必须实现的，并应用到API中去。Basic
Auth非常简单，很多框架都集成了Basic Auth的实现，自己写一个也能很快搞定，OAuth目前已经成为企业级服务的标配，其相关的开源实现方案非常丰富（更多）。

我在[《RESTful 架构风格概述》](./architectural_style.md)中，对认证机制做了更加详细的描述，有兴趣的同学不妨阅读相关章节。

权限机制是对API请求更近一步的限制，只有通过认证的用户符合权限要求，才能访问API。权限机制的具体实现通常依赖于系统的业务逻辑和应用场景，generally
speaking，常用的权限机制主要包含全局型的和对象型的，全局型的权限机制，主要指通过为用户赋予权限，或者为用户赋予角色或划分到用户组，然后为角色或用户组赋予权限的方式来实现权限控制，对象型的权限机制，主要指权限控制的颗粒度在object上，用户对某个具体对象的访问、修改、删除或其行为，要单独在该对象上为用户赋予相关权限来实现权限控制。

全局型的权限机制容易理解，实现也简单，有很多开源库可做备选方案，不少完善的web开发框架，也会集成相关的权限逻辑，object
permission 相对难复杂一点，但也有很多典型的应用场景，如多人博客系统中，作者对自己文章的编辑权限即为object
permission，其对应的开源库也有很多。

开发一套完整的RESTful API，权限机制必须纳入考虑范围，虽然权限机制的具体实现依赖于业务，权限机制本身，是有典型的模式存在的，需要开发者掌握基本的权限机制实现方案，以便随时应用到API中去。

## 5. CORS

CORS，即[Cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)，在RESTful
API开发中，主要是为js服务的，解决javascript 调用 RESTful API时的跨域问题。

由于固有的安全机制，js的跨域请求时是无法被服务器成功响应的。现在前后端分离日益成为web开发主流方式的大趋势下，后台逐渐趋向指提供API服务，为各客户端提供数据及相关操作，而网站的开发全部交给前端搞定，网站和API服务很少部署在同一台服务器上并使用相同的端口，js的跨域请求时普遍存在的，开发RESTful
API时，通常都要考虑到CORS功能的实现，以便js能正常使用API。

目前各主流web开发语言都有很多优秀的实现CORS的开源库，我们在开发RESTful API时，要注意CORS功能的实现，直接拿现有的轮子来用即可。

更多关于CORS的介绍，有兴趣的同学可以查看阮一峰老师的[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

## 6. URL Rules

RESTful API 是写给开发者来消费的，其命名和结构需要有意义。因此，在设计和编写URL时，要符合一些规范。Url rules
可以单独写一篇博客来详细阐述，本文只列出一些关键点。

### 6.1 Version your API

规范的API应该包含版本信息，在RESTful API中，最简单的包含版本的方法是将版本信息放到url中，如：

```
/api/v1/posts/
/api/v1/drafts/

/api/v2/posts/
/api/v2/drafts/
```

另一种优雅的做法是，使用HTTP header中的`accept`来传递版本信息，这也是GitHub API
采取的[策略](https://docs.github.com/zh/rest/overview/media-types?apiVersion=2022-11-28)。

### 6.2 Use nouns, not verbs

RESTful API 中的url是指向资源的，而不是描述行为的，因此设计API时，应使用名词而非动词来描述语义，否则会引起混淆和语义不清。即：

```
# Bad APIs
/api/getArticle/1/
/api/updateArticle/1/
/api/deleteArticle/1/
```

上面四个url都是指向同一个资源的，虽然一个资源允许多个url指向它，但不同的url应该表达不同的语义，上面的API可以优化为：

```
# Good APIs
/api/Article/1/
```

article 资源的获取、更新和删除分别通过`GET`，`PUT`和`DELETE`方法请求API即可。试想，如果url以动词来描述，用`PUT`
方法请求`/api/deleteArticle/1/`会感觉多么不舒服。

### 6.3 `GET` and `HEAD` should always be safe

RFC2616已经明确指出，GET和HEAD方法必须始终是安全的。例如，有这样一个不规范的API:

```
# The following api is used to delete articles
# [GET]
/api/deleteArticle?id=1
```

试想，如果搜索引擎访问了上面url会如何？

### 6.4 Nested resources routing

如果要获取一个资源子集，采用 nested routing 是一个优雅的方式，如，列出所有文章中属于Gevin编写的文章：

```
# List Gevin's articles
/api/authors/gevin/articles/
```

获取资源子集的另一种方式是基于`filter`（见下面章节），这两种方式都符合规范，但语义不同：如果语义上将资源子集看作一个**独立的资源集合
**，则使用`nested routing`感觉更恰当，如果资源子集的获取是出于过滤的目的，则使用`filter`更恰当。

至于编写RESTful API时到底应采用哪种方式，则仁者见仁，智者见智，语义上说的通即可。

### 6.5 Filter

对于资源集合，可以通过url参数对资源进行过滤，如：

```
# List Gevin's articles
/api/articles?author=gevin
分页就是一种最典型的资源过滤。
```

### 6.6 Pagination

对于资源集合，分页获取是一种比较合理的方式。如果基于开发框架（如Django REST
Framework），直接使用开发框架中的分页机制即可，如果是自己实现分页机制，Gevin的策略是：

返回资源集合是，包含与分页有关的数据如下：

```
{
"page": 1,            # 当前是第几页
"pages": 3,           # 总共多少页
"per_page": 10,       # 每页多少数据
"has_next": true,     # 是否有下一页数据
"has_prev": false,    # 是否有前一页数据
"total": 27           # 总共多少数据
}
```

当想API请求资源集合时，可选的分页参数为：

| 参数       | 含义               |     
|----------|------------------|
| page     | 当前是第几页，默认为1      |     
| per_page | 每页多少条记录，默认为系统默认值 |     

另外，系统内还设置一个`per_page_max`字段，用于标记系统允许的每页最大记录数，当`per_page`值大于 `per_page_max` 值时，每页记录条数为 `per_page_max`。

### 6.7 Url design tricks

#### （1）Url是区分大小写的，这点经常被忽略，即：
```
/Posts
/posts
```
上面这两个url是不同的两个url，可以指向不同的资源

#### （2）Back forward Slash (/)

目前比较流行的API设计方案，通常建议url以/作为结尾，如果API GET请求中，url不以/结尾，则重定向到以/结尾的API上去（这点现在的web框架基本都支持），因为有没有
/，也是两个url，即：
```
/posts/
/posts
```
这也是两个不同的url，可以对应不同的行为和资源

（3）连接符 `-` 和 下划线 `_`

RESTful API 应具备良好的可读性，当url中某一个片段（segment）由多个单词组成时，建议使用 `-` 来隔断单词，而不是使用 `_`，即：

```
# Good
/api/featured-post/

# Bad
/api/featured_post/
```

这主要是因为，浏览器中超链接显示的默认效果是，文字并附带下划线，如果API以_隔断单词，二者会重叠，影响可读性。

## 总结

编写本文的初衷，是为了整理一套从零开始编写规范、安全的RESTful API的基本思路。本文介绍了开发RESTful API时，要考虑的基本内容，对于类似 Flask 这种天生支持 RESTful 风格的web框架，不依赖其他RESTful第三方库开发RESTful
服务时，可以从本文内容入手；不少强大的RESTful 库，虽然其相关接口基本涵盖了本文的全部或大部分内容，但本文的总结，相信对这些库的理解和使用也是有帮助的。

最后，关于如何开发RESTful API，欢迎大家与我交流~


> 原文地址
> 
> https://blog.igevin.info/posts/restful-api-get-started-to-write/
