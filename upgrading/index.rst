升级 NodeBB
======================

NodeBB 定期发布版本于 `发布版 <https://github.com/NodeBB/NodeBB/releases>`_。 这些发布版包含高质量的代码，可用于生产环境部署。

你可以使用 git 安装指定版本的 NodeBB，以及周期性升级到新发布版。

如需获得最新的修订和特性，你也可以使用 ``git clone`` 直接从代码库(``master`` 分支) 克隆代码，不过这样不能保证程序的稳定性。核心开发者会在工作环境上，验证每次代码提交，虽然个别特性还没 100% 完成。

***一如既往***， NodeBB 团队不会为，可能由于升级引起的，任何意外、数据丢失、数据损坏、或者任何坏的情况负责。所以请在升级之前，**不要忘记备份**！

Upgrade Path
-------------------

NodeBB's upgrade path is designed so that upgrading between versions is straightforward. NodeBB will provide upgrade compatibility (via the ``--upgrade`` flag) between the latest version of a lower branch and the latest version of the higher branch. For example, if ``v0.2.2`` is the latest version in the ``v0.2.x`` branch, you can switch to the ``v0.3.x`` branch and suffer no ill effects. Upgrading from ``v0.2.0`` to ``v0.3.x`` is not supported, and NodeBB will warn you when attempting to upgrade that you are not upgrading cleanly.

Upgrading between patch revisions
^^^^^^^^^^^^^^^^^^^^^^^^^

*e.g. v0.1.0 to v0.1.1*

Patch revisions contain bugfixes and other minor changes. Updating to the latest version of code for your specific version branch is all that is usually required.

**Execute steps 1 through 3.**

Upgrading between minor revisions
^^^^^^^^^^^^^^^^^^^^^^^^^

*e.g. v0.1.3 to v0.2.0*

Minor revisions contain new features or substantial changes that are still backwards compatible. They may also contain dependent packages that require upgrading, and other features may be deprecated (but would ideally still be supported).

Execute steps 1 through 4.

..  (the block below was commented out in original, so I'm leaving it commented out)
	Upgrading between major revisions
	^^^^^^^^^^^^^^^^^^^^^^^^^

	*e.g. v0.2.4 to v1.0.0*

	Major revisions contain breaking changes that are done in a backwards incompatible manner. Complete rewrites of core functionality are not uncommon. In all cases, NodeBB will attempt to provide migration tools so that a transition is possible.

	Execute all of the steps.

Upgrade Steps
-------------------

**Note**: After upgrading between revisions (i.e. v0.0.4 to v0.0.5), it may be necessary to run the following upgrade steps to ensure that any data schema changes are properly upgraded as well:

1. Shut down your forum
^^^^^^^^^^^^^^^^^^^^^^^^^

While it is possible to upgrade NodeBB while it is running, it is definitely not recommended, particularly if it is an active forum:

.. code:: bash

	$ cd /path/to/nodebb
	$ ./nodebb stop


2. Back up your data
^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: 

	This section is incomplete, please take care to back up your files properly!


Backing up Redis
~~~~~~~~~~~~~~

As with all upgrades, the first step is to **back up your data**! Nobody likes database corruption/misplacement.

All of the textual data stored in NodeBB is found in a ``.rdb`` file. On typical installs of Redis, the main database is found at ``/var/lib/redis/dump.rdb``.

**Store this file somewhere safe.**

Backing up MongoDB
~~~~~~~~~~~~~~

To run a backup of your complete MongoDB you can simply run

    mongodump

which will create a directory structure that can be restored with the `mongorestore` command.

It is recommended that you first shut down your database. On Debian / Ubuntu it's likely to be: `sudo service mongodb stop`

Backing up LevelDB
~~~~~~~~~~~~~~

As LevelDB is simply a collection of flat files, just copy the database over to a safe location, ex.

.. code:: bash

    cp -r /path/to/db /path/to/backups

**Store this file somewhere safe.**

Avatars
~~~~~~~~~~~~~~

Uploaded images (avatars) are stored in /public/uploads. Feel free to back up this folder too:

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
