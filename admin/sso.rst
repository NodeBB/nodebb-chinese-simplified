社交网络 SSOs
==================

通过第三方插件，NodeBB 支持整合 Facebook, Twitter 和 Google：

* `npm install nodebb-plugin-sso-facebook`
* `npm install nodebb-plugin-sso-twitter`
* `npm install nodebb-plugin-sso-google`

也支持其他的 SSO，例如 GitHub。所有 SSO 插件，请查看 `插件目录 <http://community.nodebb.org/category/7/nodebb-plugins>`_。

在安装并激活插件后，它们需要 API 密钥才能工作：

Facebook
---------

通过 `Facebook 开发者 <https://developers.facebook.com/>`_ 页面注册应用。可能需要使用信用卡或者移动电话号码，来创建开发者帐号：

创建新应用，然后获得应用密码钥和应用密码：

.. image:: http://i.imgur.com/hfy0eVo.png

确保 "Website with Facebook Login" 被选中, 并且您 NodeBB 的 URL 在 "Site URL" 中指定. 添加网站域名到 "App Domains" 中.

将 Application ID  和 Secret 粘贴到 ACP 中相应的位置 （通过您的 NodeBB install / admin访问）

Twitter
---------

通过 `Twitter 开发者 <https://dev.twitter.com/>`_ 页面. 创建一个新的应用, 并且获得 Access Token 和 Secret：

.. image:: http://i.imgur.com/ksrHkgN.png

**重要**: 在设置应用程序时，一定要指定一个回调的地址。 这个地址不一定非要和您的安装地址一致，但是不能为空。

将 Application ID  和 Secret 粘贴到 ACP 中相应的位置 （通过您的 NodeBB install / admin访问）

Google
---------

通过 `Google API 控制台 <https://code.google.com/apis/console/>`_, 一个新的应用, 并且获得 Client ID 和 Secret：

.. image:: http://i.imgur.com/xutDs1R.png

将 Application ID  和 Secret 粘贴到 ACP 中相应的位置 （通过您的 NodeBB install / admin访问）
