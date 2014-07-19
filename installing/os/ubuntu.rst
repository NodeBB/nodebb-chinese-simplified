
Ubuntu
--------------------

首先，我们安装基础软件包:

.. code:: bash

	$ sudo apt-get install git nodejs redis-server imagemagick npm


如果你想使用 MongoDB, LevelDB, 或者其他数据库来代替 Redis, 请查阅 :doc:`Configuring Databases <../configuring/databases>` 部分.

**如果你的软件管理器中安装的 Node.js 低于 0.8 (例如 Ubuntu 12.10, 13.04), 请执行 ``node --version`` 来确认你的 Node.js 版本:**


.. code:: bash

	$ sudo add-apt-repository ppa:chris-lea/node.js
	$ sudo apt-get update && sudo apt-get dist-upgrade


接着，克隆这个库:


.. code:: bash

	$ git clone git://github.com/NodeBB/NodeBB.git nodebb


获取所有 NodeBB 需要的依赖包:

.. code:: bash

    $ cd nodebb
    $ npm install


通过运行带有 ``setup`` 标记的程序来初始化安装代码:


.. code:: bash

	$ ./nodebb setup


默认设置能用于本地服务器的缺省端口，并且redis的存储也使用了同样的机器和端口。

最后，我们运行论坛.


.. code:: bash

	$ ./nodebb start


NodeBB 也可以通过一些帮助程序来启动, 例如 ``supervisor`` 和 ``forever``. :doc:`去这里看看 <../../running/index>`.
