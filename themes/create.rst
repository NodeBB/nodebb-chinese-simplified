创建新的 NodeBB 主题
===========================

NodeBB 是基于 `Twitter Bootstrap <twitter.github.com/bootstrap/>`_ 构建的。Bootstrap 使得主题制作非常简单。

为 NodeBB 打包
-------------------------------------

NodeBB 期望通过 ``npm`` 安装任何主题。每个单独的主题是一个 npm 包，用户可以使用命令行来安装主题，例如：

.. code:: bash

    npm install nodebb-theme-modern-ui

主题的文件夹下必须包含至少两个文件，才认为是合法的主题。

1. ``theme.json``

2. ``theme.less``

``theme.less`` 是存放主题样式的地方。NodeBB 期望这个文件使用 LESS 编写，并且可以按需要预编译为CSS。如需查看更多 LESS 的信息，请访问 `LESS 的主页 <http://lesscss.org/>`_。

**提示**: A *suggested* organization for ``theme.less`` is to ``@import`` multiple smaller files instead of placing all of the styles in the main ``theme.less`` file.

配置
-------------------------------------
The theme configuration file is a simple JSON string containing all appropriate meta data regarding the theme. Please take note of the following properties:

* ``id``: A unique id for a theme (e.g. "my-theme")
* ``name``: A user-friendly name for the theme (e.g. "My Theme")
* ``description``: A one/two line description about the theme (e.g. "This is the theme I made for my personal NodeBB")
* ``screenshot``: A filename (in the same folder) that is a preview image (ideally, 370x250, or an aspect ratio of 1.48:1)
* ``url``: A fully qualified URL linking back to the theme's homepage/project

子主题
-------------------------------------

If your theme is based off of another theme, simply modify your LESS files to point to the other theme as a base:

topic.less
^^^^^^^^^^

.. code: css

    @import "../nodebb-theme-vanilla/topic";

    .topic .main-post {
        .post-info {
            font-size: 20px;  // My theme specific override
        }
    }

As ``topic.less`` from the theme ``nodebb-theme-vanilla`` was imported, those styles are automatically incorporated into your theme.

**Important**: If you depend on another theme, make sure that your theme specifically states this in its ``package.json``. For example, for the above theme, as we depend on ``nodebb-theme-vanilla``, we would explicitly state this by adding a new section into the ``package.json`` file:

.. code:: json

    "peerDependencies": {
        "nodebb-theme-vanilla": "~0.0.1"
    }
