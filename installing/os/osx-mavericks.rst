OSX Mavericks
==========

依赖
---------------------

首先，安装下面的程序:

* http://nodejs.org/
* http://brew.sh/




运行 NodeBB
---------------------

使用 homebrew 安装 redis:

.. code::

  brew install redis
  
启动 redis 服务器，在终端输入:
  
.. code::

  redis-server

克隆 NodeBB 库:

.. code:: bash

    git clone -b v0.6.x https://github.com/NodeBB/NodeBB.git

进入目录: 

.. code:: bash

    cd NodeBB

安装依赖:

.. code:: bash

    npm install

执行交互安装程序:

.. code:: bash

    ./nodebb setup

你可以全部使用默认选项。

然后搞定！在安装结束后，执行

.. code:: bash

    ./nodebb start

可以在地址 ``http://localhost:4567/`` 查看论坛


