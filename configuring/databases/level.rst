LevelDB
=======

根据你的操作系统，按照 :doc:`安装指南 <../../installing/os>` 进行安装，跳过安装 Redis 一节。

在克隆 NodeBB 代码后，你需要运行：

.. code::

    npm install levelup leveldown


最后，创建一个目录存储 LevelDB 数据库，例如：

.. code::

    mkdir /var/level/

运行 NodeBB，在提示数据库时，选择 ``level``。如果目录使用上面例子的路径，后面的配置可以使用缺省值。
