安装与概述
==================

有两种方式开始一个IdentityServer项目：

* 从头开始，完全自己写
* 在VS中使用ASP.NET Identity模板

如果你是从头完全自己写的话，我们提供了一些列的helper和内存存储，因此你不要担心持续化的问题。

如果你使用的是ASP.NET Identity， 我们提供了一个非常简单的与之集成的方法。

每个quickstart都有对应的solution - 你可以在
`IdentityServer4.Samples <https://github.com/IdentityServer/IdentityServer4.Samples>`_
仓储的quickstarts文件夹中找到。

安装
^^^^^^^^^^^
下图显示的是VS - 但这不是必须的。

**创建一个IdentityServer的quickstart**

首先创建一个新的ASP.NET Core项目于。

.. image:: images/0_new_web_project.png

选择"Empty"选项。

.. image:: images/0_empty_web.png

.. note:: IdentityServer目前支持ASP.NEY Core1.1。

下一步，添加`IdentityServer4` nuget包:

.. image:: images/0_nuget.png
    
或者你可以在包管理器控制台下运行下面的命令来安装`IdentityServer4`:

    "Install-Package IdentityServer4"

IdentityServers使用通常的模式去configure、add services到ASP.NET Core host中。
在``ConfigureServices``将其添加到依赖注入系统中。
在``Configure``将中间件添加到HTTP管道线中。

修改你的``Startup.cs``文件如下:

  public class Startup
  {
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
      loggerFactory.AddConsole();

      if (env.IsDevelopment())
      {
        app.UseDeveloperExceptionPage();
      }

      app.UseIdentityServer();
    }

    public void ConfigureServices(IServiceCollection services)
    {
      services.AddIdentityServer()
        .AddTemporarySigningCredential();
    }
  }

``AddIdentityServer``将IdentityServer服务注入到DI中。同时注册了一个内存存储器用来存储运行时的状态。这在开发场景中很有用。 For production scenarios you need a persistent or shared store like a database or cache for that.
See the :ref:`EntityFramework <refEntityFrameworkQuickstart>` quickstart for more information.

The ``AddTemporarySigningCredential`` extension creates temporary key material for signing tokens on every start.
Again this might be useful to get started, but needs to be replaced by some persistent key material for production scenarios.
See the :ref:`cryptography docs <refCrypto>` for more information.

.. Note:: IdentityServer is not yet ready to be launched. We will add the required services in the following quickstarts.

Modify hosting
^^^^^^^^^^^^^^^

By default Visual Studio uses IIS Express to host your web project. This is totally fine,
except that you won't be able to see the real time log output to the console.

IdentityServer makes extensive use of logging whereas the "visible" error message in the UI
or returned to clients are deliberately vague.

We recommend to run IdentityServer in the console host. 
You can do this by switching the launch profile in Visual Studio.
You also don't need to launch a browser every time you start IdentityServer - you can turn that off as well:

.. image:: images/0_launch_profile.png

When you switch to self-hosting, the web server port defaults to 5000. 
You can configure this either in the launch profile dialog above, or programmatically in ``Program.cs`` - 
we use the following configuration for the IdentityServer host in the quickstarts::

    public class Program
    {
        public static void Main(string[] args)
        {
            Console.Title = "IdentityServer";

            var host = new WebHostBuilder()
                .UseKestrel()
                .UseUrls("http://localhost:5000")
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }

.. Note:: We recommend to configure the same port for IIS Express and self-hosting. This way you can switch between the two without having to modify any configuration in your clients.

How to run the quickstart
^^^^^^^^^^^^^^^^^^^^^^^^^
As mentioned above every quickstart has a reference solution - you can find the code in the 
`IdentityServer4.Samples <https://github.com/IdentityServer/IdentityServer4.Samples>`_
repo in the quickstarts folder.

The easiest way to run the individual parts of a quickstart solution is to set the startup mode to "current selection".
Right click the solution and select "Set Startup Projects":

.. image:: images/0_startup_mode.png

Typically you start IdentityServer first, then the API, and then the client. Only run in the debugger if you actually want to debug.
Otherwise Ctrl+F5 is the best way to run the projects.
