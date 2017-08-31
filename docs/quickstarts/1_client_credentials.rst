.. _refClientCredentialsQuickstart:
使用客户凭证( Client Credentials )来保护AP
==========================================

quickstart展示了使用IdentityServer保护API的最基本的场景。

我们将定义一个API和一个想使用这个API的client。
client将像IdentityServer请求一个access token, 然后使用它来访问API。

定义API
^^^^^^^^^^^^^^^^
在你的系统中定义你想要保护的资源，eg: API。

因为我们在此是使用in-memory configuration - 因此你只需要添加一个API，创建一个 ``ApiResource``对象并设置其适当的属性。

在你的项目于中添加一个文件(e.g. ``Config.cs``)，代码如下::

    public static IEnumerable<ApiResource> GetApiResources()
    {
        return new List<ApiResource>
        {
            new ApiResource("api1", "My API")
        };
    }

定义client
^^^^^^^^^^^^^^^^^^^
下一步是定义一个要访问这个API的client。

在这个例子中，client没有user， and will authenticate
using the so called client secret with IdentityServer.
添加下面的代码在你的configuration中::

    public static IEnumerable<Client> GetClients()
    {
        return new List<Client>
        {
            new Client
            {
                ClientId = "client",

                // 没有interactive user, 使用clientid/secret来验证
                AllowedGrantTypes = GrantTypes.ClientCredentials,

                // 验证的secret
                ClientSecrets =
                {
                    new Secret("secret".Sha256())
                },

                // 定义能方位的scopes
                AllowedScopes = { "api1" }
            }
        };
    }

配置IdentityServer
^^^^^^^^^^^^^^^^^^^^^^^^
To configure IdentityServer to use your scopes and client definition, you need to add code
to the ``ConfigureServices`` method. 
You can use convenient extension methods for that - 
under the covers these add the relevant stores and data into the DI system::

    public void ConfigureServices(IServiceCollection services)
    {
        // configure identity server with in-memory stores, keys, clients and resources
        services.AddIdentityServer()
            .AddTemporarySigningCredential()
            .AddInMemoryApiResources(Config.GetApiResources())  // 声明所拥有的API资源
            .AddInMemoryClients(Config.GetClients());           // 声明想访问API资源的client
    }

好了 - 如果你运行服务，打开
``http://localhost:5000/.well-known/openid-configuration``, 你将看到下图的一个页面，这个页面被称为discovery document。
clients和APIS使用它下载一些必要的配置数据。

.. image:: images/1_discovery.png

添加API
^^^^^^^^^^^^^
下一步是在你的解决方案中添加一个API。

可以使用ASP.NET Core Web API template, 或者在的项目于中添加``Microsoft.AspNetCore.Mvc``包。
Again, we recommend you take control over the ports and use the same technique as you used
to configure Kestrel and the launch profile as before.
在这假设你配置你的API运行在``http://localhost:5001``.

**controller**

在你的API项目中添加一个controller::

    [Route("identity")]
    [Authorize]
    public class IdentityController : ControllerBase
    {
        [HttpGet]
        public IActionResult Get()
        {
            return new JsonResult(from c in User.Claims select new { c.Type, c.Value });
        }
    }


**Configuration**

最后一步是将authentication middleware添加到你的API host中。
这个中间件的作用是：

* 验证token，确保其来自可信的发行商（issuer）
* 验证token，看他是否能访问要访问的api

在项目中添加`IdentityServer4.AccessTokenValidation`包。

.. image:: images/1_nuget_accesstokenvalidation.png

同样需要将这个中间件添加到pipeline中。
必须在MVC前添加，eg::

    public void Configure(IApplicationBuilder app, ILoggerFactory loggerFactory)    
    {
        loggerFactory.AddConsole(Configuration.GetSection("Logging"));
        loggerFactory.AddDebug();

        app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions
        {
            Authority = "http://localhost:5000",
            RequireHttpsMetadata = false,

            ApiName = "api1"
        });

        app.UseMvc(); // 在这之前添加
    }

打开浏览器导航到``http://localhost:5001/identity`` 页面，将返回401，这意味这你需要一个凭证（credential）。

好了，你的API已经被IdentityServer保护起来了。

创建client
^^^^^^^^^^^^^^^^^^^
最后一步是写一个client，它将请求一个access token，然后用这个token来访问API。在解决方案中添加一个console项目。

The token endpoint at IdentityServer implements the OAuth 2.0 protocol, and you could use 
raw HTTP to access it. 我们有一个client library `IdentityModel`， that
encapsulates the protocol interaction in an easy to use API.

在你的应用中添加`IdentityModel`包

.. image:: images/1_nuget_identitymodel.png

IdentityModel includes a client library to use with the discovery endpoint.
This way you only need to know the base-address of IdentityServer - the actual
endpoint addresses can be read from the metadata::

    // discover endpoints from metadata
    var disco = await DiscoveryClient.GetAsync("http://localhost:5000");

Next you can use the ``TokenClient`` class to request the token.
To create an instance you need to pass in the token endpoint address, client id and secret.

Next you can use the ``RequestClientCredentialsAsync`` method to request a token for your API::

    // request token
    var tokenClient = new TokenClient(disco.TokenEndpoint, "client", "secret");
    var tokenResponse = await tokenClient.RequestClientCredentialsAsync("api1");

    if (tokenResponse.IsError)
    {
        Console.WriteLine(tokenResponse.Error);
        return;
    }

    Console.WriteLine(tokenResponse.Json);


.. note:: Copy and paste the access token from the console to `jwt.io <https://jwt.io>`_ to inspect the raw token.

The last step is now to call the API.

To send the access token to the API you typically use the HTTP Authorization header.
This is done using the ``SetBearerToken`` extension method::

    // call api
    var client = new HttpClient();
    client.SetBearerToken(tokenResponse.AccessToken);

    var response = await client.GetAsync("http://localhost:5001/identity");
    if (!response.IsSuccessStatusCode)
    {
        Console.WriteLine(response.StatusCode);
    }
    else
    {
        var content = await response.Content.ReadAsStringAsync();
        Console.WriteLine(JArray.Parse(content));
    }

The output should look like this:

.. image:: images/1_client_screenshot.png

.. note:: By default an access token will contain claims about the scope, lifetime (nbf and exp), the client ID (client_id) and the issuer name (iss).

Further experiments
^^^^^^^^^^^^^^^^^^^

This walkthrough focused on the success path so far

* client was able to request token
* client could use the token to access the API

You can now try to provoke errors to learn how the system behaves, e.g.

* try to connect to IdentityServer when it is not running (unavailable)
* try to use an invalid client id or secret to request the token
* try to ask for an invalid scope during the token request
* try to call the API when it is not running (unavailable)
* don't send the token to the API
* configure the API to require a different scope than the one in the token
