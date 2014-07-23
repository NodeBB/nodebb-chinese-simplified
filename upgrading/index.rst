升级 NodeBB
======================

NodeBB 定期发布版本于 `发布版 <https://github.com/NodeBB/NodeBB/releases>`_。 这些发布版包含高质量的代码，可用于生产环境部署。

你可以使用 git 安装指定版本的 NodeBB，以及周期性升级到新发布版。

如需获得最新的修订和特性，你也可以使用 ``git clone`` 直接从代码库(``master`` 分支) 克隆代码，不过这样不能保证程序的稳定性。核心开发者会在工作环境上，验证每次代码提交，虽然个别特性还没 100% 完成。

***一如既往***， NodeBB 团队不会为，可能由于升级引起的，任何意外、数据丢失、数据损坏、或者任何坏的情况负责。所以请在升级之前，**不要忘记备份**！

升级路径
-------------------

NodeBB 的升级路径设计为，在不同版本之间升级是直接的。NodeBB 会提供高版本分支和低版本分支直接的升级兼容 (通过 ``--upgrade`` 标记)。例如， 如果 ``v0.2.2`` 是 ``v0.2.x`` 分支的最新版本，你可以无痛切换到 ``v.0.3.x`` 分支'。而从 ``v0.2.0`` 升级到 ``v0.3.x`` 是不支持的，同时 NodeBB 会在你尝试升级时，警告升级不正确。

在补丁版本间升级
^^^^^^^^^^^^^^^^^^^^^^^^^

*例如，从 v0.1.0 升级到 v0.1.1*

Patch revisions contain bugfixes and other minor changes. Updating to the latest version of code for your specific version branch is all that is usually required.

**执行第1步到第3步。**

Upgrading between minor revisions
^^^^^^^^^^^^^^^^^^^^^^^^^

*例如，从 v0.1.3 升级到 v0.2.0*

Minor revisions contain new features or substantial changes that are still backwards compatible. They may also contain dependent packages that require upgrading, and other features may be deprecated (but would ideally still be supported).

Execute steps 1 through 4.

..  (the block below was commented out in original, so I'm leaving it commented out)
	Upgrading between major revisions
	^^^^^^^^^^^^^^^^^^^^^^^^^

	*e.g. v0.2.4 to v1.0.0*

	Major revisions contain breaking changes that are done in a backwards incompatible manner. Complete rewrites of core functionality are not uncommon. In all cases, NodeBB will attempt to provide migration tools so that a transition is possible.

	Execute all of the steps.

升级步骤
-------------------

**提示**: After upgrading between revisions (i.e. v0.0.4 to v0.0.5), it may be necessary to run the following upgrade steps to ensure that any data schema changes are properly upgraded as well:

1. 关闭你的论坛
^^^^^^^^^^^^^^^^^^^^^^^^^

While it is possible to upgrade NodeBB while it is running, it is definitely not recommended, particularly if it is an active forum:

.. code:: bash

	$ cd /path/to/nodebb
	$ ./nodebb stop


2. 备份你的数据
^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: 

	此节未完成，请正确备份你的文件！


备份 Redis
~~~~~~~~~~~~~~

对于所有的升级，第一步就是 **备份你的数据** ！没人喜欢数据库损坏。

NodeBB中，所有的文本数据都在 ``.rdb`` 文件中。在通常安装的 Redis 上，主数据库在  ``/var/lib/redis/dump.rdb``。

**Store this file somewhere safe.**

备份 MongoDB
~~~~~~~~~~~~~~

To run a backup of your complete MongoDB you can simply run

    mongodump

which will create a directory structure that can be restored with the `mongorestore` command.

It is recommended that you first shut down your database. On Debian / Ubuntu it's likely to be: `sudo service mongodb stop`

备份 LevelDB
~~~~~~~~~~~~~~

As LevelDB is simply a collection of flat files, just copy the database over to a safe location, ex.

.. code:: bash

    cp -r /path/to/db /path/to/backups

**Store this file somewhere safe.**

头像
~~~~~~~~~~~~~~

已上传的图片 (头像) 保存在 /public/uploads。请备份目录：

.. code:: bash

    cd /path/to/nodebb/public
    tar -czf ~/nodebb_assets.tar.gz ./uploads

3. Grab the latest and greatest code
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Navigate to your NodeBB: ``$ cd /path/to/nodebb``.

If you are upgrading from a lower branch to a higher branch, switch branches as necessary. ***Make sure you are completely up-to-date on your current branch!***.

For example, if upgrading from ``v0.3.2`` to ``v0.4.3``:

.. code:: bash

    $ git fetch    # Grab the latest code from the NodeBB Repository
    $ git checkout v0.4.x    # Type this as-is! Not v0.4.2 or v0.4.3, but "v0.4.x"!
    $ git merge origin/v0.4.x

If not upgrading between branches, just run the following command:

.. code:: bash

    $ git pull

This should retrieve the latest (and greatest) version of NodeBB from the repository.

Alternatively, download and extract the latest versioned copy of the code from `the Releases Page <https://github.com/NodeBB/NodeBB/releases>`_. Overwrite any files as necessary. This method is not supported.

4. Run the NodeBB upgrade script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This script will install any missing dependencies, upgrade any plugins or themes (if an upgrade is available), and migrate the database if necessary.

.. code:: bash

    $ ./nodebb upgrade

**Note**: ``./nodebb upgrade`` is only available after v0.3.0. If you are running an earlier version, run these instead:

* ``npm install``
* ``ls -d node_modules/nodebb* | xargs -n1 basename | xargs npm update``
* ``node app --upgrade``

6. Start up NodeBB & Test!
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You should now be running the latest version of NodeBB.
