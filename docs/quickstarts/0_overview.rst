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

``AddIdentityServer``将IdentityServer服务注入到DI中。同时注册了一个内存存储器用来存储运行时的状态。这在开发场景中很有用。在生产环境下你需要使用database或者缓存来持续化这些数据。
更多请见 :ref:`EntityFramework <refEntityFrameworkQuickstart>` quickstart for more information.

The ``AddTemporarySigningCredential`` extension creates temporary key material for signing tokens on every start.
Again this might be useful to get started, but needs to be replaced by some persistent key material for production scenarios.
See the :ref:`cryptography docs <refCrypto>` for more information.

.. Note:: IdentityServer is not yet ready to be launched. We will add the required services in the following quickstarts.

修改hosting
^^^^^^^^^^^^^^^

VS默认使用IIS Express来host你的Web项目。这样做没什么问题。
但是这样的话，你不能在console里面看到实时输出的log。

IdentityServer makes extensive use of logging whereas the "visible" error message in the UI
or returned to clients are deliberately vague.

我们推荐使用console host来运行IdentityServer。
你可以在VS的lauch profile里面的切换。
同样不需要在每次运行IdentityServer的时候运行浏览器 - 你可以关闭它：

.. image:: images/0_launch_profile.png

当你切换到self-hosting, Web Server的端口默认为5000。
你可以在launch profile对话框中进行配置，或者在``Program.cs``中使用代码进行配置 - 
在quickstarts中使用下面的代码来配置IdentityServer host::

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

.. Note:: 推荐配置IIS Express和self-hosting为同一个端口。这样你不需要在切换它们的时候去修改配置。

怎么运行quickstart
^^^^^^^^^^^^^^^^^^^^^^^^^
上面提到了每个quickstarts都有一个解决方案 - 能在
`IdentityServer4.Samples <https://github.com/IdentityServer/IdentityServer4.Samples>`_
仓储中的qucikstarts文件夹中看到。

运行quickstart最简单的方法：设置启动模式为"current selection"。
右键解决方案，选择 "Set Startup Projects":

.. image:: images/0_startup_mode.png

通常先启动IdentityServer，然后是API，在后是client。只有在debug的时候才以debugger模式运行。
