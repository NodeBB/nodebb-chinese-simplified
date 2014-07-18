SmartOS
========

要求
----------------

在安装 NodeBB 之前，需要安装以下软件:

* Node.js 的版本不低于 0.8 或者以上
* Redis 的版本 2.6 或者以上 (下面会讲解如何从 Joyent's package 库来安装).
* nginx, 的版本 1.3.13 或者以上 (**除非** 希望用 nginx 向 NodeBB 服务器发代理需求).

服务器访问
----------------

1. 登陆你的 Joyent 帐号: `Joyent.com <http://joyent.com>`_

2. 选择: ``Create Instance``

3. 创建最新的 ``smartos nodejs`` 镜像.  

    **注意:** 以下步骤在镜像 ``smartos nodejs 13.1.0`` 进行过测试。

4. 等待你的 instance 显示 `Running` 时，点击它的名字

5. 找到 ``登陆`` 以及 admin 密码. 如果缺失 ``Credentials`` 部分, 请刷新页面。  

    **Example:** ``ssh root@0.0.0.0`` ``A#Ca{c1@3`` 

6. 使用 admin 而不是 root 通过 SSH 方式连接到你的服务器: ``ssh admin@0.0.0.0``  

    **注意:** 对于没有安装 ssh 的 Windows 用户, 这里可以选择用: `Cygwin.com <http://cygwin.com>`_

安装
----------------

1. 安装 NodeBB 软件依赖包:

    .. code:: bash

        $ sudo pkgin update
        $ sudo pkgin install scmgit nodejs build-essential ImageMagick redis

    如果有任何一个失败:

    .. code:: bash

        $ pkgin search *failed-name*
        $ sudo pkgin install *available-name*

2. **如果需要** 采用默认设置安装 redis-server 并设置为服务 (自动启动和重启):  
    **注意:** 这些步骤可以快速安装 redis server 但并没有调整到使用状态。  

    **注意:** 如果你手动运行 `redis-server`， 那么现在就可以退出了.  

    .. code:: bash

        $ svcadm enable redis
        $ svcs

    *-* 如果 `svcs` 显示 "/pkgsrc/redis:default" 在维护状态，那么:

    .. code:: bash

        $ scvadm clear redis  

    *-* 关闭你的 redis-server 并避免重启:

    .. code:: bash

        $ scvadm disable redis

    *-* 启动 redis-server 并保证它一直运行:

    .. code:: bash

        $ scvadm enable redis

3. 移动到你想要创建 nodebb 文件夹的地方:

    .. code:: bash

        $ cd /parent/directory/of/nodebb/

4. 克隆 NodeBB 库:

    .. code:: bash

        $ git clone git://github.com/NodeBB/NodeBB.git nodebb

5. 安装 NodeBB 的 npm 依赖包:

    .. code:: bash

        $ cd nodebb/
        $ npm install

6. 执行 NodeBB 安装脚本:  

    .. code:: bash

        $ node app --setup

    A. `这里的安装URL` 要么是你从 ssh 登陆的 ip 地址，要么是指向 ip 地址的域名。  

        **例如:** `http://0.0.0.0` or `http://example.org`  

    B. `你的 NodeBB` 端口号在访问网站时是必须的:  

        **注意:** 如果你没有用 nginx 来代理端口，那么推荐使用80端口。 
    C. 如果你是按照以上步骤安装 redis-server，那么请使用默认的 redis 设置。 

7. 启动 NodeBB 过程:  

    **手动启动 NodeBB :**

    **注意:** 日常不要这样用.  

    .. code:: bash

        $ node app

8. 访问网站!  
    **例如:** 端口为 4567: ``http://0.0.0.0:4567`` or ``http://example.org:4567``

    **注意:** 如果端口是 `:80` 就不需要输入。 

**注意:** 如果这些你觉得这些说明不够清楚或者你在运行时遇到了问题，请通过发起问题 <https://github.com/NodeBB/NodeBB/issues>`_ 让我们知道

升级 NodeBB
----------------

**注意:** 详细的升级说明请参考 :doc:`Upgrading NodeBB <../../upgrading/index>`.
