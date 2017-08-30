欢迎来到IdentityServer4
==========================

.. image:: images/logo.png
   :align: center

IdentityServer4是一个ASP.NET Core下面的OpenID Connect和OAuth 2.0的框架。

能让你的应用有下面的一些功能：

将认证服务化
^^^^^^^^^^^^^^^^^^^^^^^^^^^
为你的多个应用（web, native, mobile,services）提供集中化的登录逻辑。

单点登录
^^^^^^^^^^^^^^^^^^^^^^^^^
为多个应用提供单点登录。

为API提供进入控制
^^^^^^^^^^^^^^^^^^^^^^^
为服务于多种客户端类型（server to server, web applications, SPAs和native/mobile 应用）的API提供进入tokens。

Federation Gateway
^^^^^^^^^^^^^^^^^^
支持外部的identity provider（如Azure Active Directory, Google, Facebook等）。
This shields your applications from the details of how to connect to these external providers.

可定制
^^^^^^^^^^^^^^^^^^^^^^
最重要的是 - IdentityServer支持定制化以适配你专有的需求。
因为IdentityServer是一个框架，不是一个封闭的产品或SaaS，你可以对IdentityServer进行定制以适配你的需求和场景。

免费的商业支持
^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you need help building or running your identity platform, :ref:`let us know <refSupport>`.
There are several ways we can help you out.

IdentityServer is officially certified by the OpenID Foundation and part of the .NET Foundation.

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Introduction

   intro/big_picture
   intro/architecture
   intro/terminology
   intro/specs
   intro/packaging
   intro/support
   intro/test
   intro/contributing

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Quickstarts

   quickstarts/0_overview
   quickstarts/1_client_credentials
   quickstarts/2_resource_owner_passwords
   quickstarts/3_interactive_login
   quickstarts/4_external_authentication
   quickstarts/5_hybrid_and_api_access
   quickstarts/6_aspnet_identity
   quickstarts/7_javascript_client
   quickstarts/8_entity_framework
   quickstarts/community

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Configuration

   configuration/startup
   configuration/resources
   configuration/clients
   configuration/mvc
   configuration/apis

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Topics

   topics/startup
   topics/resources
   topics/clients
   topics/signin
   topics/signin_external_providers
   topics/windows
   topics/signout
   topics/signout_external_providers
   topics/signout_federated
   topics/consent
   topics/apis
   topics/deployment
   topics/logging
   topics/events
   topics/crypto
   topics/grant_types
   topics/secrets
   topics/extension_grants
   topics/resource_owner
   topics/refresh_tokens
   topics/reference_tokens
   topics/cors
   topics/discovery
   topics/add_protocols
   topics/tools

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Endpoints

   endpoints/discovery
   endpoints/authorize
   endpoints/token
   endpoints/userinfo
   endpoints/introspection
   endpoints/revocation
   endpoints/endsession

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Reference

   reference/identity_resource
   reference/api_resource
   reference/client
   reference/grant_validation_result
   reference/interactionservice
   reference/options

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Misc

   misc/training
   misc/blogs
   misc/videos
