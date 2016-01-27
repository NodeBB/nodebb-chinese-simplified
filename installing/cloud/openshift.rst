Openshift Paas
===========

下面所列的是关于Pass平台 `Openshift <http://openshift.com>` 的相关安装说明.

**步骤 1:** 创建新应用 :

.. code:: bash
	
	rhc app create nodebb nodejs-0.10

**步骤 2:** 添加Redis拓展

.. code:: bash
	
	rhc add-cartridge http://cartreflect-claytondev.rhcloud.com/reflect?github=smarterclayton/openshift-redis-cart -a nodebb

**步骤 3:** 使用SSH连接应用

.. code:: bash
	
	rhc app ssh -a nodebb
	
**步骤 4:** 找出您实例的IP地址以便Nodejs和redis能正确绑定. 这是Openshift的要求,并且似乎只有这样才能正常工作. 你不能使用 $IP 即使是在你的 config.json 上也不行 (也就是说你不能在NodeApp-setup上输入$IP).第一行 : NodeJS 第二行 : Redis
宏 $REDIS_CLI 在屏幕上打印的内容应该是这样的 : -h ip_redis -p port_redis -a password

.. code:: bash

  echo $OPENSHIFT_NODEJS_IP && echo $REDIS_CLI
  
**步骤 5:** 退出SSH

**步骤6 6:** 添加NodeBB源代码到代码库中

.. code:: bash
	
	cd nodebb && git remote add upstream -m master git://github.com/NodeBB/NodeBB.git

**步骤 7:** 获得文件并且Push

.. code:: bash
	
	git pull -s recursive -X theirs upstream master && git push
	
**步骤 8:** 停止应用程序

.. code:: bash
	
	rhc app stop -a nodebb

**步骤 9:** 使用SSH重连到应用程序

.. code:: bash
	
	rhc app ssh -a nodebb

**步骤 10:** 在SSH终端上编辑NodeJS运行环境

.. code:: bash
	
	cd ~/nodejs/configuration && nano node.env
	
**步骤 11:** 用 app.js 替换 server.js 然后退出编辑器

.. code:: bash
	
	ctrl + x
	
**步骤 12:** 在其他终端,启动应用

.. code:: bash
	
	rhc app start -a nodebb

**步骤 13:** 在SSH终端上启动NodeBB安装向导

.. code:: bash
	
	cd ~/app-root/repo && node app --setup

安装向导的链接应该为 'http://nodebb-username.rhcloud.com', 请替换username为您设置的Openshift后缀. 

端口号 : 8080

被绑定的主机名或IP: 在此处输入在步骤4中的您的 $OPENSHIFT_NODEJS_IP 值.

您的MongoDB实例的IP地址: 此处为在步骤4中的您的 $REDIS_CLI  值.

您的MongoDB实例的IP端口: 此处为在步骤4中的您的 $REDIS_CLI  值.

Redis 密码: 此处为在步骤4中的您的 $REDIS_CLI  值.

**步骤 14:** 最后一个啦~呼~!在SS终端重启应用

.. code:: bash
	
	rhc app restart -a nodebb

然后在浏览器中打开 http://nodebb-username.rhcloud.com.

提醒
---------------------------------------
不要偷懒哟~在OP面板上重启NodeBB无效的,务必使用 :

.. code:: bash
	
	rhc app restart -a nodebb
