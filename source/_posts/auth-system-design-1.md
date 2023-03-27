---
title: 认证授权系统设计【一】相关概念或术语
date: 2023-03-26 21:21:09
categories:
- system-design
tags:
- Authentication & Authorization
---

# OAuth

## 什么是 OAuth
> OAuth 是一个关于授权的开放网络标准，
> 允许用户让第三方应用访问该用户在某一网站上存储的私密的资源（如照片，视频，联系人列表），
> 而无需将用户名和密码提供给第三方应用。

### OAuth 协议中定义的角色

* ”资源所有者（resource owner）“：一个有能力准予访问一个受保护资源的实体。当资源所有者是一个人时，其称为最终用户（end-user）。有时缩写为 ”RO“。
* ”资源服务器（resource server）“：托管受保护资源的服务器，有能力接受并且响应使用访问令牌的受保护资源请求。常常通过一个API可访问资源服务器。有时缩写为”RS“。
* ”客户端（client）“：一个因为资源所有者和他的授权发起受保护资源的应用程序。术语”客户端（client）“不能指代任何一个具体的实现特征（例：应用是否运行在一个服务器，一个台式计算机或者其他设备上）。
* ”授权服务器（authorization server）“： 在资源所有者成功授权以及取得权限之后给客户端发放访问令牌的服务器。有时缩写为”AS“。


### OAuth 工作流程
<div align="center">
<pre>
+--------+                               +---------------+
|        |--(A)- Authorization Request ->|   Resource    |
|        |                               |     Owner     |
|        |<-(B)-- Authorization Grant ---|               |
|        |                               +---------------+
|        |
|        |                               +---------------+
|        |--(C)-- Authorization Grant -->| Authorization |
| Client |                               |     Server    |
|        |<-(D)----- Access Token -------|               |
|        |                               +---------------+
|        |
|        |                               +---------------+
|        |--(E)----- Access Token ------>|    Resource   |
|        |                               |     Server    |
|        |<-(F)--- Protected Resource ---|               |
+--------+                               +---------------+
</pre>
</div>

* （A）客户端向从资源所有者请求授权。授权请求可以直接向资源所有者发起（如图所示），或者更可取的是通过作为中介的授权服务器间接发起。
* （B）客户端收到授权许可，这是一个代表资源所有者的授权的凭据，使用本规范中定义的四种许可类型之一或 者使用扩展许可类型表示。授权许可类型取决于客户端请求授权所使用的方式以及授权服务器支持的类型。
* （C）客户端与授权服务器进行身份认证并出示授权许可请求访问令牌。
* （D）授权服务器验证客户端身份并验证授权许可，若有效则颁发访问令牌。
* （E）客户端从资源服务器请求受保护资源并出示访问令牌进行身份验证。
* （F）资源服务器验证访问令牌，若有效则满足该请求

### OAuth2.1 带来了哪些变化

* ⚡ 推荐使用 Authorization Code + PKCE
* ⚡ 【已弃用】隐式授权（Implicit Grant）
* ⚡ 【已弃用】密码授权 （Resource Owner Password Credentials Grant）
* ⚡ 使用 access_token 时, 不应该通过 URL 传递 token
* ⚡ 刷新令牌 (Refresh Token) 应该是一次性的
* ⚡ 回调地址（Redirect URI）应该精确匹配

#### 推荐使用 Authorization Code + PKCE
下边是 Authorization Code + PKCE 的授权流程图

![](https://blog-1259586045.cos.ap-shanghai.myqcloud.com/clipboard_20211113_103034.png)

## 参考

* [OAuth 2.1 带来了哪些变化](https://www.cnblogs.com/myshowtime/p/15596630.html)
* [IETF OAuth 2.1 Draft - 08](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-v2-1-08)