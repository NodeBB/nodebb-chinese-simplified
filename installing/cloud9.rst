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

安装使用的 URL 应设置为 'http://workspace_name-c9-username.c9.io', 用你的空间名替换 workspace_name ，username 与你注册的一致。 请注意 NodeBB 当前采用未加密的 http 来读取 jQuery，是因为使用 http:// 比 使用 https:// 更为方便。 否则 jQuery 不会加载， NodeBB 也会崩溃。

端口号不是那么重要 - Cloud9 可能你强制使用80端口，建议按此设置。如果你想设置为其他端口，例如4567，那也是可以的。 

使用端口号来访问 NodeBB? 没有设么太大的区别，设置为 No。如果不这样设置也是可以的。

Redis 进程的主机 IP: localhost ($IP 命令的结果也可以用)

要绑定的 IP 地址或主机名: Enter what your $IP value holds here found in 输入步骤 4中 $IP 命令的结果，类似于: 123.4.567.8

Redis 进程的主机端口: 16379

Redis 密码: 除非你手动设置密码，Redis 默认不会设置密码。这项留空或直接按 Enter

第一次运行时也需要设置一个 Admin 名称，email 地址 和密码。

到底这里你已经大功告成了! 不要使用 IDE 顶部的 Run 按钮, 建议使用命令行。运行:

.. code:: bash
	
	node app

然后在浏览器中打开 http://workspace_name-c9-username.c9.io

疑难解答
---------------

最常见的问题是数据库没有启动. 请确定已经正确设置了 Redis，然后运行 ：

.. code:: bash
	
	redis-server --port 16379 --bind $IP
