Image 存储服务 APIs
======================


启用 Imgur 图片上传
----------------------------

帖子要支持图片附件，首先创建申请 imgur 应用:

https://api.imgur.com/oauth2/addclient

你可以使用 : "Anonymous usage without user authorization" (无用户验证的匿名使用方式)

然后你会得到一个 "Client ID". 

然后安装 nodebb-plugin-imgur:

.. code::
	
	npm install nodebb-plugin-imgur

在控制面板中激活插件，然后重启 NodeBB。

你应该在控制面板中看到 Imgur 菜单项了。把 Client ID 粘贴到插件页面上的 "Imgur Client ID" 中。保存，然后你就可以在编辑器窗口拖拽图片来上传了。



上传到 Amazon S3
-----------------------

.. note:: 

	现在没有文档，更多信息请查看帖子 `the plugin thread <https://community.nodebb.org/topic/796/nodebb-plugin-s3-uploads-store-your-uploads-in-aws-s3>`_。