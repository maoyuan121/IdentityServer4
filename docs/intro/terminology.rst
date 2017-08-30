术语
===========

文档和对象模型使用了一些术语，你需要有所了解。

.. image:: images/terminology.png

IdentityServer
^^^^^^^^^^^^^^
IdentityServer是一个OpenID Connect Provider - 它实现了OpenID Conect和OAuth 2.0 protocal。

不同的文献使用了不同的术语 - security token service, identity provider, authorization server, IP-STS等。

But they are in a nutshell all the same: a piece of software that issues security tokens to clients.

IdentityServer有斜面的一些功能：

* 保护你的资源

* 使用本地的account store认证用户， 或通过外部的identity provider认证用户

* 提供session管理和单点登录

* manage and authenticate clients

* issue identity and access tokens to clients

* validate tokens

用户
^^^^
用户是一个人，通过注册成客户来访问资源。

Client
^^^^^^
Client是一些软件，它从IdentityServer请求一个token - either for authenticating a user (requesting an identity token) 
or for accessing a resource (requesting an access token). A client must be first registered with IdentityServer before it can request tokens.

Examples for clients are web applications, native mobile or desktop applications, SPAs, server processes etc.

资源
^^^^^^^^^
Resource是你想使用IdentityServer保护的东西 - either identity data of your users, or APIs. 

Every resource has a unique name - and clients use this name to specify to which resources they want to get access to.

**Identity data**
Identity information (aka claims) about a user, e.g. name or email address.

**APIs**
APIs resources represent functionality a client wants to invoke - typically modelled as Web APIs, but not necessarily.

Identity Token
^^^^^^^^^^^^^^
An identity token represents the outcome of an authentication process. It contains at a bare minimum an identifier for the user 
(called the `sub` aka subject claim) and information about how and when the user authenticated.  It can contain additional identity data.

Access Token
^^^^^^^^^^^^
An access token allows access to an API resource. Clients request access tokens and forward them to the API. 
Access tokens contain information about the client and the user (if present).
APIs use that information to authorize access to their data.
