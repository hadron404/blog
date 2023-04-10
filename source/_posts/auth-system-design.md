---
title: 认证授权系统设计【零】认证、授权、鉴权和权限控制
date: 2023-03-25 20:37:01
categories:
- system-design
tags:
- Authentication & Authorization
---

## Authentication：认证

### 概念
首先来看一下英文中的对 Authentication 的解释：

[Cambridge Dictionary](https://dictionary.cambridge.org/dictionary/english/authentication)
> the process of proving that something is real, true, or what people say it is

[Merriam-webster Dictionary](https://www.merriam-webster.com/dictionary/authentication)
> : an act, process, or method of showing something (such as an identity, a piece of art, 
> or a financial transaction) to be real, true, or genuine <br>
> : the act or process of authenticating something

[Wikipedia](https://en.wikipedia.org/wiki/Authentication)
> Authentication is the act of proving an assertion, 
> such as the identity of a computer system user. 
> In contrast with identification, the act of indicating a person or thing's identity, 
> authentication is the process of verifying that identity.
> It might involve validating personal identity documents, 
> verifying the authenticity of a website with a digital certificate,
> determining the age of an artifact by carbon dating, 
> or ensuring that a product or document is not counterfeit.

它通常会与 Authorization 和 Authenticity 混淆，**简单地说，验证用户身份的过程，
按照国际惯例称为 身份验证**，也可以简称为 **认证**（以下均以认证称之）。

### 目的
认证的目的是确保只有合法的用户才能访问系统和资源。

### 主要任务和其实现时的注意点
#### 主要任务
* 收集用户身份验证信息
* 验证用户身份
* 记录身份验证结果
* 处理身份验证错误

#### 注意点
###### 安全性
认证系统需要确保用户身份验证信息的安全性。这包括保护用户信息的隐私和防止未经授权的访问
###### 性能
认证系统需要确保高性能，以便在大量用户同时访问系统时仍能快速验证用户的身份
###### 可伸缩性
认证系统需要具备可伸缩性，以便随着用户数量的增长而扩展
###### 可靠性
认证系统需要确保可靠性，以避免出现故障或停机时间过长，导致用户无法访问系统

### 常见的认证方案
* HTTP Basic Authentication
* 基于 Session 的认证
  * 基于单体架构的 Session 方案
  * 基于分布式架构的 Session 方案
* 基于 Token 的认证
  * 基于 JWT 的方案
  * 基于 OAuth 的方案

### 常规的认证形式
1. 账号密码认证
2. 短信认证
3. 第三方 IDP 认证
   1. Google 
   2. Github
   3. QQ

   
## Authorization：授权
### 概念
[Cambridge Dictionary](https://dictionary.cambridge.org/dictionary/english/authorization)
> official permission for something to happen, 
> or the act of giving someone official permission to do something:<br>
> Medical records cannot be disclosed without authorization from the patient.<br>
> [ + to infinitive ] The authorization to sell the shares arrived too late.

[Merriam-webster Dictionary](https://www.merriam-webster.com/dictionary/authorization)
> : the act of authorizing<br>
> : an instrument that authorizes : SANCTION

[Wikipedia](https://en.wikipedia.org/wiki/Authorization)
> Authorization or authorisation (see spelling differences) is the function of specifying access rights/privileges to resources, 
> which is related to general information security and computer security, 
> and to access control in particular.

_Authorization（授权）是确定用户能够访问哪些资源的过程。_

一般是在 **Authentication（认证） 成功之后，根据用户身份和权限规则， 决定用户是否有权访问所请求的资源或执行某些操作的过程**

### 目的
授权的目的是确保用户只能访问其被授权的资源，而不能访问未经授权的资源。


### 主要任务和实现时注意点
#### 主要任务
* 确定用户的身份和权限
* 检查用户是否有权访问权限
* 记录授权结果
* 处理授权错误

#### 注意点
###### 安全性
授权系统需要确保资源的安全性，包括对访问资源的限制和保护，以避免未经授权的访问
###### 精细控制
授权系统需要实现细粒度的访问控制，以确保每个用户仅访问其被授权的资源
是授权(Authorization）中的一部分，用于决定用户是否可以访问某个资源或执行某个操作。
常用的访问控制模型，两种 RBAC ：
* 基于资源的权限访问控制（Resource-based access control，RBAC）
* 基于角色的权限访问控制（Role-based access control，RBAC）
###### 可伸缩性
授权系统需要具备可伸缩性，以便随着用户数量的增长而扩展
###### 可靠性
授权系统需要确保可靠性，以避免出现故障或停机时间过长，导致用户无法访问系统

## Authentication and Authorization：鉴权
### 概念

#### 鉴

>  ◎ 镜子。<br>
>  ◎ 照：光可鉴人。<br>
>  ◎ 观察，审察：鉴别。鉴定。鉴赏。鉴于（看到，觉察到）。台鉴（书信用语，表示请人看信。亦作“惠鉴”、“钧鉴”）。鉴往知来。<br>
>  ◎ 可以使人警惕或引为教训的事情：借鉴。鉴戒。前车之鉴<br>

#### 权

> ◎ 职责范围内支配和指挥的力量：政权。权力。权威。权贵。权柄。权势。生杀予夺之权。<br>
> ◎ 有利的形势：主动权。<br>
> ◎ 变通，不依常规：权变。权谋（随机应变的计谋）。权术。智必知权。<br>
> ◎ 暂且，姑且：权且。<br>
> ◎ 秤锤：权衡。<br>
> ◎ 衡量，估计：权其轻重。<br>
> ◎ 姓。<br>

这里主要指的是权力了，在计算机世界中，权力就是访问资源的权力了，即权限。
实际是认证和授权两个过程的合并，称之为 **鉴权** 。

### 目的
鉴权的目的是同时验证用户的身份和授权信息，以决定用户是否有权访问所请求的资源。一般是先认证，后授权，
整个认证授权的过程在中文语境下称之为鉴权。英文语境下是 Authentication and Authorization 。 
比如 [JAAS](https://zh.wikipedia.org/wiki/JAAS)， Java Authentication and Authorization Server。


### 常见的技术实现方案

* 客户端 Token 与 API 网关结合
* 客户端 Token 方案
* 分布式 Session 方案
* Single Sign On，即单点登录，用于解决多个应用系统之间的身份认证和访问控制
  * 基于 父域 Cookie 子域共享方案
  * 基于 CAS 的实现
  * 基于 OAuth 的实现

## 权限控制
权限控制是指对可执行的各种操作组合配置为权限列表，然后根据执行者的权限，若其操作在权限范围内，
则允许执行，否则禁止。权限控制在英文中对应于 access/permission control 。