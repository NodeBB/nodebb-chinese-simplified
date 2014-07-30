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

通过 `Facebook 开发者 <https://developers.facebook.com/>`_ 页面注册应用。可能需要使用信用卡或者移动电话号码，来创建开发者帐号。

创建新应用，然后获得应用密码钥和应用密码：

.. image:: http://i.imgur.com/hfy0eVo.png

Ensure that "Website with Facebook Login" is checked, and that the URL to your NodeBB instance is specified in the "Site URL" box. Add that site's domain to the "App Domains" field.

Paste this key and secret into the appropriate boxes in the NodeBB Administration Panel (accessible via /admin on your NodeBB install)

Twitter
---------

Register an application at the `Twitter Developers <https://dev.twitter.com/>`_ page. Create a new Application, and obtain the Access Token and Secret:

.. image:: http://i.imgur.com/ksrHkgN.png

**Important**: While setting up your application, be sure to specify a Callback URL. It does not have to correspond to your installation, it just cannot be blank.

Paste this token and secret into the appropriate boxes in the NodeBB Administration Panel (accessible via /admin on your NodeBB install)

Google
---------

Register an application at the `Google API Console <https://code.google.com/apis/console/>`_, and obtain a Client ID and Secret.

.. image:: http://i.imgur.com/xutDs1R.png

Paste this ID and secret into the appropriate boxes in the NodeBB Administration Panel (accessible via /admin on your NodeBB install)
