编写 NodeBB 插件
==========================

这么说，您要给 NodeBB 写插件，棒极了！在编写插件前，有几件有帮助的事情你应该知道。

和 WordPress 类似，NodeBB 的插件搭建在 NodeBB 的钩子系统上。钩子系统，通过可控制的方式，把部分 NodeBB 功能开放给插件开发者，可以允许插件更改内容，或者在条件触发时执行指定的行为。

更多信息请查看全部 :doc:`钩子列表 <hooks>`。

过滤器、动作
------------------

有两类钩子：**过滤器** 和 **执行器**。

**过滤器** 作用于内容。当内容在 NodeBB 中传递时，你可以进行修改。例如，可以用过滤器修改帖子内容，比如把帖子中出现的所有“苹果”更改为“橙子”。类似地，可以用过滤器美化内容（比如代码过滤器），或者移除粗口（粗口过滤器）。


**执行器**会在 NodeBB 中特定的位置执行。在特定动作触发后 **做** 些事情。例如，执行器可以用来，在用户发帖时，通知管理员。其他的用法包括分析记录，或者在新用户注册后自动发送欢迎的消息。


当你编写插件时，先确认你需要位置的钩子是否存在。如果不存在，`发布问题 <https://github.com/NodeBB/NodeBB/issues>`_ 我们会在下个 NodeBB 版本中加入这个的钩子。


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
The ``staticDirs`` property is an object hash that maps out paths (relative to your plugin's root) to a directory that NodeBB will expose to the public at the route ``/plugins/{YOUR-PLUGIN-ID}``.

* e.g. The ``staticDirs`` hash in the sample configuration maps ``/path/to/your/plugin/public/images`` to ``/plugins/my-plugin/images``

The ``less`` property contains an array of paths (relative to your plugin's directory), that will be precompiled into the CSS served by NodeBB.

The ``hooks`` 属性是一个数组，包含一组对象，告诉 NodeBB 你 插件使用的钩子，以及当调用钩子时，库中使用的方法。每个对象包含下面的属性 (星号标记的属性是必需的):

* ``hook``, NodeBB 钩子的名称
* ``method``, 插件中调用的方法
* ``priority``, 最终调用方法时，相对的优先级 (默认: 10)

编写插件库
------------------

您插件的核心是库文件，当您的插件激活是，会自动引入 NodeBB。
The core of your plugin is your library file, which gets automatically included by NodeBB if your plugin is activated.

Each method you write into your library takes a certain number of arguments, depending on how it is called:

* Filters send a single argument through to your method, while asynchronous methods can also accept a callback.
* Actions send a number of arguments (the exact number depends how the hook is implemented). These arguments are listed in the :doc:`list of hooks <hooks>`.

Example library method
------------------

If we were to write method that listened for the ``action:post.save`` hook, we'd add the following line to the ``hooks`` portion of our ``plugin.json`` file:

.. code:: json

    { "hook": "action:post.save", "method": "myMethod" }

Our library would be written like so:

.. code:: javascript

    var MyPlugin = {
            myMethod: function(postData) {
                // do something with postData here
            }
        };

使用 NodeBB 库增强您的插件
------------------

Occasionally, you may need to use NodeBB's libraries. For example, to verify that a user exists, you would need to call the ``exists`` method in the ``User`` class. To allow your plugin to access these NodeBB classes, use ``module.parent.require``:

.. code:: javascript

    var User = module.parent.require('./user');
    User.exists('foobar', function(err, exists) {
        // ...
    });

安装插件
------------------

In almost all cases, your plugin should be published in `npm <https://npmjs.org/>`_, and your package's name should be prefixed "nodebb-plugin-". This will allow users to install plugins directly into their instances by running ``npm install``.

When installed via npm, your plugin **must** be prefixed with "nodebb-plugin-", or else it will not be found by NodeBB.

As of v0.0.5, "installing" a plugin by placing it in the ``/plugins`` folder is still supported, but keep in mind that the package ``id`` and its folder name must match exactly, or else NodeBB will not be able to load the plugin. *This feature may be deprecated in later versions of NodeBB*.

测试
------------------

运行 NodeBB 的开发模式：

.. code::

    ./nodebb dev

这可以打印出插件的调试日志，您可以查看到，已加载的插件，注册钩子。在管理员面板中激活您的插件，然后试一下。
This will expose the plugin debug logs, allowing you to see if your plugin is loaded, and its hooks registered. Activate your plugin from the administration panel, and test it out.

禁用插件
-------------------

您可以在管理员控制面板中禁用插件，如果您的论坛由于失效的插件而崩溃，您可以通过执行下面的命令重置所有插件
You can disable plugins from the ACP, but if your forum is crashing due to a broken plugin you can reset all plugins by executing

.. code::

    ./nodebb reset plugins

或者，您可以禁用单个插件，运行下面的命令
Alternatively, you can disable one plugin by running

.. code::

    ./nodebb reset plugin="nodebb-plugin-im-broken"