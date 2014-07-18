Windows 8
==========

需要的软件
---------------------

首先，需要安装一下程序:

* https://windows.github.com/
* http://nodejs.org/
* http://sourceforge.net/projects/redis/files/redis-2.6.10/

你可能需要重启电脑。

运行 NodeBB
---------------------

启动 Redis Server

.. 注意::

	Redis Server 的默认位置是

	**C:\\Program Files (x86)\\Redis\\StartRedisServer.cmd**

打开 Git Shell, 输入以下命令. 克隆 NodeBB repo:

.. code:: bash

    git clone https://github.com/NodeBB/NodeBB.git

进入目录: 

.. code:: bash

    cd NodeBB

安装依赖:

.. code:: bash

    npm install

运行交互式安装:

.. code:: bash

    node app.js

可以默认安装过程中的所有选项。

到这里，你已经完成了安装! 现在运行

.. code:: bash

    node app.js

你可以通过 ``http://127.0.0.1:4567/`` 来访问论坛了。


在 Windows 上进行开发
---------------------

当你做了一些改变，并需要关闭和重启 NodeBB 时，可能稍微麻烦一些。首先安装 supervisor:

.. code:: bash

    npm install -g supervisor

打开 bash:

.. code:: bash

    bash

在 "watch" 模式下运行 NodeBB:

.. code:: bash

    ./nodebb watch

这样就会在开发模式下运行 NodeBB，一旦文件有所改变，会自动重启论坛。
