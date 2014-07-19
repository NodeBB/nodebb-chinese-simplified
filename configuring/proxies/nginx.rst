配置 nginx 为代理
============================

NodeBB 默认运行在 ``4567`` 端口，这意味着可以用主机名加端口来访问：

.. code::

    http://example.org:4567

为了让 NodeBB 不通过端口直接提供服务，不论 NodeBB 运行在哪个端口，都可以通过配置 nginx ，将指定主机名（或子域名）的所有请求代理转发给 NodeBB 上游。

必需组件
------------

* NGINX 版本 v1.3.13 或更高
    * 包管理器可能没有提供足够新的版本。获取最新的版本，`自行编译 <http://nginx.org/en/download.html>`_，或者 Ubuntu 上,，使用 `NGINX 稳定版 <https://launchpad.net/~nginx/+archive/stable>`_ 或者 `NGINX 开发版 <https://launchpad.net/~nginx/+archive/development>`_ PPA 构建，如果在 Debian 上，使用 `DotDeb 库 <http://www.dotdeb.org/instructions/>`_ 来获取 Nginx 的最新版本。
    * 确认你的 nginx 版本，在命令行下执行 ``nginx -V``

配置
------------

NGINX 服务的网站设置在 ``server`` 区块。基于 nginx 的安装和配置，此区块的选项有些不同的地方：

* ``/path/to/nginx/sites-available/*`` -- 这里的文件必需链接到 ``../sites-enabled``
* ``/path/to/nginx/conf.d/*.conf`` -- 文件名结尾必须是 ``.conf``
* ``/path/to/nginx/httpd.conf`` -- 必需全部配置正确，否则会启动失败

下面是基本的 nginx 配置，NodeBB 运行在 ``4567`` 端口：

.. code:: nginx

    server {
        listen 80;

        server_name forum.example.org;

        location / {
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_set_header X-NginX-Proxy true;

            proxy_pass http://127.0.0.1:4567/;
            proxy_redirect off;

            # Socket.IO Support
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }


注释
------------

* 记得要编辑 `config.json` 把 `use_port` 从 `true` 修改为 `false`
* nginx 版本高于 1.4.x 才能完全支持 websockets。Debian/Ubuntu 使用 1.2 版本，NodeBB 一样能够运行，因为有降级机制。
* 如果你的 NodeBB 在和 nginx 是用一台物理机运行，那么 ``proxy_pass`` 的 IP 应该是 ``127.0.0.1``。根据你的 NodeBB 的配置，更改匹配的的端口号。
* 这个配置能设置您的 nginx 服务器监听 ``forum.example.org`` 的请求。但它不能把互联网路由到这里，所以，你还需要更新你的 DNS 服务器，把 ``forum.example.org`` 设置为 nginx 对应的机器！