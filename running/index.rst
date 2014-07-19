运行 NodeBB
================

The preferred way to start and stop NodeBB is by invoking its executable:

* ``./nodebb start`` 启动 NodeBB 服务器
* ``./nodebb stop`` 停止 NodeBB 服务器
* 或者，你可以使用 ``npm start`` 和 ``npm stop`` 做一样的事。

The methods listed below are alternatives to starting NodeBB via the executable.


Simple Node.js Process
-----------------------

To start NodeBB, run it with ``node`` (some distributions use the executable ``nodejs``, please adjust accordingly):

.. code:: bash

    $ cd /path/to/nodebb/install
    $ node app

However, bear in mind that crashes will cause the NodeBB process to halt, bringing down your forum. Consider some of the more reliable options, below:

Supervisor Process
-----------------------

Using the `supervisor package <https://github.com/isaacs/node-supervisor>`_, you can have NodeBB restart itself if it crashes:

.. code:: bash

    $ npm install -g supervisor
    $ supervisor app

As ``supervisor`` by default continues to pipe output to ``stdout``, it is best suited to development builds.

Forever Daemon
-----------------------

Another way to keep NodeBB up is to use the `forever package <https://github.com/nodejitsu/forever>`_ via the command line interface, which can monitor NodeBB and re-launch it if necessary:

.. code:: bash

    $ npm install -g forever
    $ forever start app.js
