升级 NodeBB
======================

NodeBB 定期在 `发布版本页面 <https://github.com/NodeBB/NodeBB/releases>`_ 中发布新版。 这些发布版包含高质量的代码，可用于生产环境部署。

你可以使用 git 安装指定版本的 NodeBB，以及周期性升级到新发布版。

如需获得最新的修订和特性，你也可以使用 ``git clone`` 直接从代码库(``master`` 分支) 克隆代码，不过这样不能保证程序的稳定性。核心开发者会在工作环境上，验证每次代码提交，虽然个别特性还没 100% 完成。

**一如既往**， NodeBB 团队不会为，可能由于升级引起的，任何意外、数据丢失、数据损坏、或者任何坏的情况负责。所以在升级之前，**请不要忘记备份**！

升级路径
-------------------

NodeBB 的升级路径设计为，在不同版本之间升级是直接的。NodeBB 会提供高版本分支和低版本分支直接的升级兼容 (通过 ``--upgrade`` 标记)。例如， 如果 ``v0.2.2`` 是 ``v0.2.x`` 分支的最新版本，你可以无痛切换到 ``v.0.3.x`` 分支'。而从 ``v0.2.0`` 升级到 ``v0.3.x`` 是不支持的，同时 NodeBB 会在你尝试升级时，警告升级不正确。

在补丁版本间升级
^^^^^^^^^^^^^^^^^^^^^^^^^

*例如，从 v0.1.0 升级到 v0.1.1*

补丁版本包含错误修正和其他小的改动。需要升级到你所在小版本序列中的最新补丁版本。

**执行“升级步骤”一节的第1步到第3步。**

在小版本之间升级
^^^^^^^^^^^^^^^^^^^^^^^^^

*例如，从 v0.1.3 升级到 v0.2.0*

小版本包含一些新的特性和重要的改动，但会保持向后兼容。
其中可能涉及到依赖软件包的升级，而且其他特性有可能废弃（但是还是支持的，只会通过日志中提醒）

执行“升级步骤”一节的第1步到第4步。

..  (the block below was commented out in original, so I'm leaving it commented out)
	Upgrading between major revisions
	^^^^^^^^^^^^^^^^^^^^^^^^^

	*e.g. v0.2.4 to v1.0.0*

	Major revisions contain breaking changes that are done in a backwards incompatible manner. Complete rewrites of core functionality are not uncommon. In all cases, NodeBB will attempt to provide migration tools so that a transition is possible.

	Execute all of the steps.

升级步骤
-------------------

**提示**: 在小版本中升级后 (例如 v0.0.4 到 v0.0.5), 也有可能需要执行下面的升级步骤，以确保数据结构能正确的升级:

1. 关闭论坛
^^^^^^^^^^^^^^^^^^^^^^^^^

虽然可以在 NodeBB 运行时进行升级，但是很明显不推荐这种做法，尤其是对活跃的论坛而言。

.. code:: bash

	$ cd /path/to/nodebb
	$ ./nodebb stop


2. 备份数据
^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: 

	此节并不完整，请小心并正确地备份你的文件！


备份 Redis
~~~~~~~~~~~~~~

对于所有的升级，第一步就是 **备份你的数据** ！没人喜欢数据库损坏。

NodeBB中，所有的文本数据都在 ``.rdb`` 文件中。在通常安装的 Redis 上，主数据库在  ``/var/lib/redis/dump.rdb``。

**把文件保存到安全的地方。**

备份 MongoDB
~~~~~~~~~~~~~~

执行备份 MongoDB，只需要运行

    mongodump

此命令会创建一个目录结构的备份，而且可以通过 `mongorestore` 命令恢复备份。

推荐备份前，先关闭数据库。在 Debian / Ubuntu, 执行命令: `sudo service mongodb stop`

备份 LevelDB
~~~~~~~~~~~~~~

因为 LevelDB 是一些数据文件的简单集合，只需要把数据库复制到安全的位置，例如：

.. code:: bash

    cp -r /path/to/db /path/to/backups

**把文件存储到安全的地方。**

头像
~~~~~~~~~~~~~~

已上传的图片 (头像) 保存在 /public/uploads。请备份目录：

.. code:: bash

    cd /path/to/nodebb/public
    tar -czf ~/nodebb_assets.tar.gz ./uploads

3. 获取最新代码
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

进入 NodeBB 目录：``$ cd /path/to/nodebb``。

如果你从低版本分支升级到高版本分支，必须要切换分支。***确认你当前的分支已经更新到最新！***

例如，如果从 ``v0.3.2`` 升级到 ``v0.4.3``:

.. code:: bash

    $ git fetch    # 从 NodeBB 代码库获取最新的代码
    $ git checkout v0.4.x    # 根据需要的版本输入 v0.4.2 或者 v0.4.3 等，而不是 "v0.4.x"！
    $ git merge origin/v0.4.x

如果不是在分支之间升级，只需要执行下面的命令：

.. code:: bash

    $ git pull

从代码库获取最新(最高)版本的 NodeBB。

或者，从 `发布页面 <https://github.com/NodeBB/NodeBB/releases>`_ 下载 NodeBB 的最新版本，解压并覆盖原有文件。不推荐此方法。

4. 运行 NodeBB 升级脚本
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

脚本会安装缺失的依赖软件包，升级任何插件或主题 (如果存在新版)，视情况迁移数据库。

.. code:: bash

    $ ./nodebb upgrade

**Note**: ``./nodebb upgrade`` 只在 v0.3.0 后可用。如果你运行的是更早的版本，可运行下面的命令：

* ``npm install``
* ``ls -d node_modules/nodebb* | xargs -n1 basename | xargs npm update``
* ``node app --upgrade``

6. 启动 NodeBB、测试！
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

你现在可以运行最新版本的 NodeBB 了。
