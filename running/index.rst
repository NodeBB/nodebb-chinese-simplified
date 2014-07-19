运行 NodeBB
================

推荐的方式，是调用 NodeBB 自己的执行文件，来启动、停止 NodeBB：

* ``./nodebb start`` 启动 NodeBB 服务器
* ``./nodebb stop`` 停止 NodeBB 服务器
* 或者，你可以使用 ``npm start`` 和 ``npm stop`` 做一样的事。

下面是启动 NodeBB 的其他方法。


简单的 Node.js 进程
-----------------------

启动 NodeBB，通过 ``node`` 运行（某些发行版使用 ``nodejs``，请按情况调整）：

.. code:: bash

    $ cd /path/to/nodebb/install
    $ node app

然后, 记住异常会导致 NodeBB 进程退出，导致你的论坛宕机。可以考虑使用下面这些更可靠的方案:

Supervisor 进程
-----------------------

使用 `supervisor 包 <https://github.com/isaacs/node-supervisor>`_，你能在 NodeBB 异常时，自动重启 NodeBB：

.. code:: bash

    $ npm install -g supervisor
    $ supervisor app

缺省情况下，``supervisor`` 持续将输出通过管道发给 ``stdout``，它最适合开发版本。

Forever 后台程序
-----------------------

另一个保持 NodeBB 运行的方法，是使用 `forever 包 <https://github.com/nodejitsu/forever>`_ 。通过命令行接口，forever 可以监视 NodeBB，在需要的时候重启 NodeBB：

.. code:: bash

    $ npm install -g forever
    $ forever start app.js
