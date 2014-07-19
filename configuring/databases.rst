配置数据库
=====================

NodeBB 有数据库抽象层 (DBAL)，可以根据选择的数据库，开发对应的驱动。现在我们有下面的选项：

.. toctree::
    :hidden:
    :maxdepth: 0

    MongoDB <databases/mongo>
    LevelDB <databases/level>

* Redis (缺省, 查看 :doc:`安装指引 <../installing/os>`)
* :doc:`Mongo <databases/mongo>`
* :doc:`Level <databases/level>`

.. note::

    如果你想要为 NodeBB 写自己的数据库驱动，请访问我们的 `社区论坛 <https://community.nodebb.org>`_ 然后我们可以帮你指引正确的方向。


运行辅助数据库
----------------------------


.. warning::

    **此选项是试验性的。不应该在生产环境使用。**


Both databases **must** be flushed before beginning - there isn't a mechanism yet that detects an existing installation on one database but not another. Until fail-safe's such as these are implemented this option is hidden under the ``--advanced`` setup flag.

.. code:: bash

    node app --setup --advanced

Consult the other database guides for instructions on how to set up each specific database. Once you select a secondary database's modules, there's no turning back - until somebody writes an exporter/importer.

Currently this setup is being tested with Redis as the primary store (sets, lists, and sorted sets, because Redis is super fast with these), and Mongo as the hash store (post and user data, because ideally we wouldn't want this in RAM).
