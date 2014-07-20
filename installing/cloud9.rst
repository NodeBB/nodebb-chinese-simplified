Cloud 9 IDE
===========

以下是基于网页 `Cloud 9 <https://c9.io/>`_ IDE 的安装说明。

**步骤 1:** 从 Github 上克隆 NodeBB 到一个新的空间，你可以在终端使用以下命令:

.. code:: bash
	
	git clone git://github.com/NodeBB/NodeBB.git nodebb

Git 地址后面的 nodebb 命令会创建一个名为 nodebb 的文件夹。完成克隆后，你必须进入这个文件夹。

**步骤 2:** 用 Cloud9 的软件包管理器安装 redis

.. code:: bash
	
	nada-nix install redis

**步骤 3:** 在端口 16379 运行 redis 服务- 在 Cloud 9，6379 端口似乎已经被占用了。 "&" 可以让命令在后台运行。你可以随时终止进程. $IP 是 Cloud 9 系统变量，包含了服务器进程的全局 ip。

.. code:: bash
	
	redis-server --port 16379 --bind $IP &

**步骤 4:** 找到进程的 ip 地址并将 NodeBB 和它绑定. 这是 Cloud 9 所必要的，并且好像只有这样才能生效。你同样不能在 config.json 中使用 $IP (也就是说不能在 node app --setup 中输入 $IP).

.. code:: bash
	
	echo $IP

**步骤 5:** 安装 NodeBB 和依赖包:

.. code:: bash
	
	npm install

**步骤 6:** 运行 nodebb 安装工具:

.. code:: bash
	
	node app --setup

安装使用的 URL 应设置为 'http://workspace_name-c9-username.c9.io', 用你的空间名替换 workspace_name ，用户名不变。 Note that as NodeBB is currently using unsecure http for loading jQuery you will find it much easier using http:// instead of https:// for your base url. Otherwise jQuery won't load and NodeBB will break.

Port number isn't so important - Cloud9 may force you to use port 80 anyway. Just set it to 80. If this is another port, like 4567, that is also fine.

Use a port number to access NodeBB? Again, this doesn't seem to make a big difference. Set this to no. Either will work.

Host IP or address of your Redis instance: localhost (the output of the $IP Command is also acceptable)

IP or Hostname to bind to: Enter what your $IP value holds here found in 步骤 4. It should look something like: 123.4.567.8

Host port of your Redis instance: 16379

Redis Password: Unless you have set one manually, Redis will be configured without a password. Leave this blank and press enter

First-time set-up will also require an Admin name, email address and password to be set.

And you're good to go! Don't use the Run button at the top if the IDE, it has been a little buggy for me. Besides, you're better off using the command line anyway. Run:

.. code:: bash
	
	node app

And then open http://workspace_name-c9-username.c9.io in your browser.

Troubleshooting
---------------

A common problem is that the database hasn't been started. Make sure you have set Redis up correctly and ran 

.. code:: bash
	
	redis-server --port 16379 --bind $IP