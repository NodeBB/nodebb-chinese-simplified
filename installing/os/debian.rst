
Debian
======

当前 Ubuntu 的向导无法完全适用于 Debian。其中有一些细节特别是 NodeJS 的安装以及如何获取 Redis。

要求
^^^^^^^^^^^^^^^^^^^^^^^
在安装 NodeBB 之前，需要安装以下软件:

* Node.js 的版本不低于 0.8 或者以上
* Redis 的版本 2.6 或者以上
* 需要安装 cURL, 通过运行 ``sudo apt-get install curl`` 来进行安装

安装 Node.js 
^^^^^^^^^^^^^^^^^^^^^^^

Debian 7, Debian 6 以及更旧的版本默认没有包含 `nodejs` 软件包, 不过这里有一些办法在你的 Debian 上安装 Node.js。

Wheezy Backport :
------------------

这个方法 **仅适用于 Debian 7**, 以 **root** 身份运行以下命令 :

.. code:: bash

	$ echo "deb http://ftp.us.debian.org/debian wheezy-backports main" >> /etc/apt/sources.list
	$ apt-get update


安装 Node.js + NPM, 运行以下 :

.. code:: bash

	$ apt-get install nodejs-legacy
	$ curl --insecure https://www.npmjs.org/install.sh | bash


下面安装的 Node.js 高于 0.8 版本。(在 2014年3月29日 : 0.10.21)

从源代码编译 :
------------------

这个方法适用于 Debian 6 (Squeeze) 及更高版本, 以 **root** 身份运行以下命令 :

.. code:: bash

	$ sudo apt-get install python g++ make checkinstall
	$ src=$(mktemp -d) && cd $src
	$ wget -N http://nodejs.org/dist/node-latest.tar.gz
	$ tar xzvf node-latest.tar.gz && cd node-v*
	$ ./configure
	$ fakeroot checkinstall -y --install=no --pkgversion $(echo $(pwd) | sed -n -re's/.+node-v(.+)$/\1/p') make -j$(($(nproc)+1)) install
	$ sudo dpkg -i node_*


通过 DotDeb 安装最新软件
^^^^^^^^^^^^^^^^^^^^^^^

Dotdeb 是一个软件库，这个软件库能够让你的 Debian 变得强大、稳定，并与 LAMP 服务器保持更新。

* Nginx,
* PHP 5.4 and 5.3 (useful PHP extensions : APC, imagick, Pinba, xcache, Xdebug, XHpro..)
* MySQL 5.5,
* Percona toolkit,
* Redis,
* Zabbix,
* Passenger…

Dotdeb 支持 :

* Debian 6.0 “Squeeze“ and 7 “Wheezy“
* both amd64 and i386 architectures

Debian 7 (Wheezy) :
------------------

获得完整的 DotDeb 库 :

.. code:: bash

	$ sudo echo 'deb http://packages.dotdeb.org wheezy all' >> /etc/apt/sources.list
	$ sudo echo 'deb-src http://packages.dotdeb.org wheezy all' >> /etc/apt/sources.list


之后，添加一个 GPC keys :

.. code:: bash

	$ wget http://www.dotdeb.org/dotdeb.gpg
	$ sudo apt-key add dotdeb.gpg


更新你的软件包源 :

.. code:: bash

	$ sudo apt-get update


Debian 6 (Squeeze)
------------------

获得完整的 DotDeb 库 :

.. code:: bash

	$ sudo echo 'deb http://packages.dotdeb.org squeeze all' >> /etc/apt/sources.list
	$ sudo echo 'deb-src http://packages.dotdeb.org squeeze all' >> /etc/apt/sources.list


之后，添加一个 GPC keys :

.. code:: bash

	$ wget http://www.dotdeb.org/dotdeb.gpg
	$ sudo apt-key add dotdeb.gpg


更新你的软件包源 :

.. code:: bash

	$ sudo apt-get update


安装 NodeBB
^^^^^^^^^^^^^^^^^^^^^^^

现在，我们已经安装好了 NodeJS 并准备安装 Redis, 运行这个命令安装基础软件包 ：

.. code:: bash

	$ apt-get install redis-server imagemagick git


接着克隆这个库 :

.. code:: bash

	$ cd /path/to/nodebb/install/location
	$ git clone git://github.com/NodeBB/NodeBB.git nodebb

现在我们将通过 NPM 来安装 NodeBB 所有的依赖包 :

.. code:: bash

	$ cd /path/to/nodebb/install/location/nodebb (or if you are on your install location directory run : cd nodebb)
	$ npm install

通过运行带有 `--setup` 标记的程序来安装 NodeBB :

.. code:: bash

	$ ./nodebb setup


1. `这里的安装URL` 要么是你从 ssh 登陆的 ip 地址，要么是指向 ip 地址的域名。  
    **例如:** ``http://0.0.0.0`` or ``http://example.org``  

2. ``你的 NodeBB` 端口号在访问网站时是必须的:  
    **注意:** 如果你没有用 nginx 来代理端口，那么推荐使用80端口。 
3. 如果你是按照以上步骤安装 redis-server，那么请使用默认的 redis 设置。

做完上面之后.. 现在可以运行 NodeBB forum

.. code:: bash

	$ ./nodebb start


**注意:** 如果 NodeBB 或者服务器崩溃, NodeBB 不会自动重启 (快照), 这就是为什么你需要看看是不是通过帮助程序，例如 ``supervisor`` and ``forever`` 来启动 NodeBB。 参考:doc:`去这里看看 <../../running/index>` 点一下就进去了!

其他的，提示和建议
^^^^^^^^^^^^^^^^^^^^^^^

让你安装的 NodeBB 更安全, `去这里看看 <https://github.com/NodeBB/NodeBB#securing-nodebb>`_.

如果你希望将 NodeBB 安装在80端口，你应该使用 Nginx 设置代理  :doc:`去这里看看 <../../configuring/proxies>`
