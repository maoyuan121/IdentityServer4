Packaging and Builds
====================

IdentityServer组合了许多nuget包。

IdentityServer4
^^^^^^^^^^^^^^^
`nuget <https://www.nuget.org/packages/IdentityServer4/>`_ | `github <https://github.com/identityserver/IdentityServer4>`_

包含了IdentityServer对象模型，服务和中间件。
Only contains support for in-memory configuration and user stores - but you can plug-in support for other stores via the configuration. This is what the other repos and packages are about.

Quickstart UI
^^^^^^^^^^^^^
`github <https://github.com/IdentityServer/IdentityServer4.Quickstart.UI>`_

包含了一个简单的UI页面用来登录，登出和许可页面。

Access token validation middleware
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`nuget <https://www.nuget.org/packages/IdentityServer4.AccessTokenValidation>`_ | `github <https://github.com/IdentityServer/IdentityServer4.AccessTokenValidation>`_

ASP.NET Core中间件用来在API中验证token。提供一个简单的方法用来验证access token(both JWT and reference) and enforce scope requirements.

ASP.NET Core Identity
^^^^^^^^^^^^^^^^^^^^^
`nuget <https://www.nuget.org/packages/IdentityServer4.AspNetIdentity>`_ | `github <https://github.com/IdentityServer/IdentityServer4.AspNetIdentity>`_

这个包提供了一个简单的配置API使用ASP.NET Identity managament library for your IdentityServer users.

EntityFramework Core
^^^^^^^^^^^^^^^^^^^^
`nuget <https://www.nuget.org/packages/IdentityServer4.EntityFramework>`_ | `github <https://github.com/IdentityServer/IdentityServer4.EntityFramework>`_

EntityFramework Core用来为IdentityServer提供数据持久化功能。
This package provides an EntityFramework implementation for the configuration and operational stores in IdentityServer.

Dev builds
^^^^^^^^^^
In addition we publish dev/interim builds to MyGet.
Add the following feed to your Visual Studio if you want to give them a try:

https://www.myget.org/F/identity/
