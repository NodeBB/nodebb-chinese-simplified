编写 NodeBB 插件
==========================

这么说，您要给 NodeBB 写插件，棒极了！在编写插件前，有几件有帮助的事情你应该知道。

和 WordPress 类似，NodeBB 的插件搭建在 NodeBB 的钩子系统上。钩子系统，通过可控制的方式，把部分 NodeBB 功能开放给插件开发者，可以允许插件更改内容，或者在条件触发时执行指定的行为。

更多信息请查看全部 :doc:`钩子列表 <hooks>`。

过滤器、动作
------------------

有两类钩子：**过滤器** 和 **执行器**。

**过滤器** 作用于内容。当内容在 NodeBB 中传递时，你可以进行修改。例如，可以用过滤器修改帖子内容，比如把帖子中出现的所有“苹果”更改为“橙子”。类似地，可以用过滤器美化内容（比如代码过滤器），或者移除粗口（粗口过滤器）。


**执行器** 会在 NodeBB 中特定的位置执行。在特定动作触发后 **做** 些事情。例如，执行器可以用来，在用户发帖时，通知管理员。其他的用法包括分析记录，或者在新用户注册后自动发送欢迎的消息。


当你编写插件时，先确认你需要位置的钩子是否存在。如果不存在，`提交一个申请 <https://github.com/NodeBB/NodeBB/issues>`_，然后我们会在下个 NodeBB 版本中加入这个的钩子。


配置
------------------

每个插件包含一个配置文件，文件名 ``plugin.json``。下面是个例子：

.. code:: json

    {
        "id": "my-plugin",
        "name": "我的酷毙插件",
        "description": "您插件的描述",
        "url": "您插件的地址或者 Github 代码库",
        "library": "./my-plugin.js",
        "staticDirs": {
            "images": "public/images"
        },
        "less": [
            "assets/style.less"
        ],
        "hooks": [
            { "hook": "filter:post.save", "method": "filter" },
            { "hook": "action:post.save", "method": "emailme" }
        ]
    }

``id`` 属性是唯一的名称，用来标识插件。

``library`` 属性是库相对插件包目录的路径。NodeBB 会自动加载库（如果插件处于激活状态）。

``staticDirs`` 属性
``staticDirs`` 属性是对象的散列，映射对外的路径（相对你插件的根目录）到一个目录，然后 NodeBB 会将这个公开，并映射到 ``/plugins/{YOUR-PLUGIN-ID}``。

* 例如，在一个示例配置中， ``staticDirs`` 散列映射 ``/path/to/your/plugin/public/images`` 到 ``/plugins/my-plugin/images``

``less`` 属性包含一组路径（相对你插件的目录），会预编译进入 NodeBB 的 CSS。

The ``hooks`` 属性是一个数组，包含一组对象，告诉 NodeBB 你 插件使用的钩子，以及当调用钩子时，库中使用的方法。每个对象包含下面的属性 (星号标记的属性是必需的):

* ``hook``, NodeBB 钩子的名称
* ``method``, 插件中调用的方法
* ``priority``, 最终调用方法时，相对的优先级 (默认: 10)

编写插件库
------------------

你插件的核心是库文件，当您的插件激活时，NodeBB 会自动引用。

你在库中编写的每个方法都有确定的参数个数，取决于怎样调用：

* 过滤器发送单个参数给你的方法，异步方法也可以接受回调。
* 执行器发送一些参数（具体个数取决于钩子的实现）。这些参数在:doc:`钩子列表 <hooks>`的文档中列出。

库方法示例
------------------

如果我们要写个方法，用来监听 ``action:post.save`` 钩子，我们应该添加下面的行到 ``plugin.json`` 文件的 ``hooks`` 部分：

.. code:: json

    { "hook": "action:post.save", "method": "myMethod" }

我们的库应该这样写：

.. code:: javascript

    var MyPlugin = {
            myMethod: function(postData) {
                // 在这里处理 postData
            }
        };

使用 NodeBB 库增强您的插件
------------------

偶尔，你可能需要使用 NodeBB 的库。例如，检查用户是否存在，你需要调用 ``User`` 类的 ``exists`` 方法。使用 ``module.parent.require``，来启用你的插件访问这些 NodeBB 类：

.. code:: javascript

    var User = module.parent.require('./user');
    User.exists('foobar', function(err, exists) {
        // ...
    });

安装插件
------------------

绝大多数情况下，你的插件应该发布在 `npm <https://npmjs.org/>`_ 上，然后你的包名应该已 "nodebb-plugin-" 开头。这样可以让用户，通过运行 ``npm install`` 把插件直接安装到他们的实例中。

当通过 npm 安装时，你的插件 **必须** 已 "nodebb-plugin-" 开头，否则 NodeBB 会找不到它。

v0.0.5 版中，把插件放入 ``/plugins`` 目录来进行"安装"，依然是支持的。但是需要注意的是，包的 ``id`` 和它所在目录的名称必须是完全匹配的，否则 NodeBB 不能加载它。*这个特性已在 NodeBB 的最新版本中废弃*。

测试
------------------

运行 NodeBB 的开发模式：

.. code::

    ./nodebb dev

这可以打印出插件的调试日志，你可以查看到，已加载的插件，插件注册的钩子。在管理员面板中激活你的插件，然后测试一下。

禁用插件
-------------------

你可以在管理员控制面板中禁用插件，如果你的论坛由于失效的插件而崩溃，可以通过执行下面的命令重置所有插件。

.. code::

    ./nodebb reset plugins

或者，你可以禁用单个插件，运行下面的命令

.. code::

    ./nodebb reset plugin="nodebb-plugin-im-broken"