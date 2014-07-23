创建新的 NodeBB 主题
===========================

NodeBB 是基于 `Twitter Bootstrap <twitter.github.com/bootstrap/>`_ 构建的。Twitter Bootstrap 让开发主题更加简单。

为 NodeBB 打包
-------------------------------------

NodeBB 期望通过 ``npm`` 安装任何主题。每个单独的主题是一个 npm 包，用户可以使用命令行来安装主题，例如：

.. code:: bash

    npm install nodebb-theme-modern-ui

主题的文件夹下必须包含至少两个文件，才认为是合法的主题。

1. ``theme.json``

2. ``theme.less``

``theme.less`` 是存放主题样式的地方。NodeBB 期望这个文件使用 LESS 编写，并且可以按需要预编译为CSS。如需查看更多 LESS 的信息，请访问 `LESS 的主页 <http://lesscss.org/>`_。

**提示**: 对 ``theme.less`` 内容组织的 *建议*，使用 ``@import`` 语法来导入多个小文件，而不是把所有样式都放入 ``theme.less`` 文件中。

配置
-------------------------------------
主题配置文件是一个简单的 JSON 字符串。其中包含主题相关的元数据。请注意下面的属性：

* ``id``: 主题的唯一 ID (例如 "my-theme")
* ``name``: 主题的用户友好的名称 (例如 "我的主题")
* ``description``: 一两行关于主题的描述 (e.g. "这是个我为 NodeBB 私人定制的主题")
* ``screenshot``: 文件名 (和配置在同一个目录下)，预览图片 (最好尺寸 370x250，或者长宽比 1.48:1)
* ``url``: 主题主页/项目的 URL 链接

子主题
-------------------------------------

如果你的主题是基于其他主题修改的，可以简单修改你的 LESS
文件指向其他主题：

topic.less
^^^^^^^^^^

.. code: css

    @import "../nodebb-theme-vanilla/topic";

    .topic .main-post {
        .post-info {
            font-size: 20px;  // 我主题的覆盖参数
        }
    }

当 ``topic.less`` 从主题 ``nodebb-theme-vanilla`` 导入是，这些样式会自动整合到你的主题中。

**重要**：如果你依赖别的主题，确认你的主题在 ``package.json`` 中指定依赖关系。例如，上面的例子中，我们依赖 ``nodebb-theme-vanilla``，我们应该在 ``package.json`` 中显式的添加依赖关系：

.. code:: json

    "peerDependencies": {
        "nodebb-theme-vanilla": "~0.0.1"
    }
