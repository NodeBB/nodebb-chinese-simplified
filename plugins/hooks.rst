可用的钩子
=============

下面是 NodeBB 中现有的所有钩子的列表。此列表作为插件开发者的手册。更多信息，请查看 :doc:`编写 NodeBB 插件 <create>`.

共有两类钩子，**过滤器**，和 **动作** 。过滤器处理单个输入 (提供了一个参数)，按某种方式解析后，返回修改后的值。动作处理多个输入，执行的动作由接受的输入决定。动作没有返回值。

**重要**: 此列表并不详尽。需要的时候会增加新的钩子 (或者我们在今后能看到使用示例)，所有添加新钩子的需求应该通过 `问题跟踪 <https://github.com/NodeBB/NodeBB/issues>`_ 发给我们。


过滤器
----------

``filter:admin.header_build``
^^^^^^^^^^^^^^^^^^^^^

运行插件在 ACP 中创建新的导航链接

``filter:post.save``
^^^^^^^^^^^^^^^^^^^^^

**参数**: 帖子内容 (markdown 文本)

当帖子创建或者编辑时，写入数据库之前执行。

``filter:post.get``
^^^^^^^^^^^^^^^^^^^^^

**参数**: 帖子对象 (javascript 对象)

帖子从数据库取回后，发送到客户端之前执行。

``filter:header.build``
^^^^^^^^^^^^^^^^^^^^^

**允许插件在 NodeBB 中添加新的导航链接**

``filter:register.build``
^^^^^^^^^^^^^^^^^^^^^

**参数**:
 - `req` express 请求对象 (javascript 对象)
 - `res` express 响应对象 (javascript 对象)
 - `data` 传递给模板的数据 (javascript 对象)

**允许插件在注册表单中添加新的元素。现在，支持持 `data.captcha`**


``filter:post.parse``
^^^^^^^^^^^^^^^^^^^^^

**参数**: 帖子或者签名档原始文本 (字符串)

当帖子或签名档，从原始文本解析为 HTML (输出给客户端的内容) 时执行。可用调用更漂亮的解析器，例如 `Markdown <http://daringfireball.net/projects/markdown/>`_，或者 `BBCode <http://www.bbcode.org/>`_。

``filter:posts.custom_profile_info``
^^^^^^^^^^^^^^^^^^^^^

**允许插件，在主题作者的帖子区块中，添加自定义的资料信息**


``filter:register.check``
^^^^^^^^^^^^^^^^^^^^^

**参数**:
 - `req` express 请求对象 (javascript 对象)
 - `res` express 响应对象 (javascript 对象)
 - `userData` 从表单解析的用户数据

**允许用户检查信息，并且在需要时拒绝注册。**


``filter:scripts.get``
^^^^^^^^^^^^^^^^^^^^^

**允许在头部添加客户端 JS，生产环境会自动进行压缩处理**


``filter:uploadImage``
^^^^^^^^^^^^^^^^^^^^^

``filter:uploadFile``
^^^^^^^^^^^^^^^^^^^^^

``filter:widgets.getAreas``
^^^^^^^^^^^^^^^^^^^^^

``filter:widgets.getWidgets``
^^^^^^^^^^^^^^^^^^^^^

``filter:search.query``
^^^^^^^^^^^^^^^^^^^^^

``filter:post.parse``
^^^^^^^^^^^^^^^^^^^^^

``filter:messaging.save``
^^^^^^^^^^^^^^^^^^^^^^^^

``filter:messaging.parse``
^^^^^^^^^^^^^^^^^^^^^

``filter:sounds.get``
^^^^^^^^^^^^^^^^^^^^^

``filter:post.getPosts``
^^^^^^^^^^^^^^^^^^^^^

``filter:post.getFields``
^^^^^^^^^^^^^^^^^^^^^

``filter:auth.init``
^^^^^^^^^^^^^^^^^^^^^

``filter:composer.help``
^^^^^^^^^^^^^^^^^^^^^

``filter:topic.thread_tools``
^^^^^^^^^^^^^^^^^^^^^

``filter:user.create``
^^^^^^^^^^^^^^^^^^^^^

``filter:user.delete``
^^^^^^^^^^^^^^^^^^^^^

``filter:user.profileLinks``
^^^^^^^^^^^^^^^^^^^^^

``filter:user.verify.code``
^^^^^^^^^^^^^^^^^^^^^
参数: confirm_code

Ability to modify the generated verification code (ex. for using a shorter verification code instead for SMS verification)

``filter:user.custom_fields``
^^^^^^^^^^^^^^^^^^^^^

Parameters: userData

Allows you to append custom fields to the newly created user, ex. mobileNumber

``filter:register.complete``
^^^^^^^^^^^^^^^^^^^^^
Parameters: uid, destination

Set the post-registration destination, or do post-register tasks here.

``filter:widget.render``
^^^^^^^^^^^^^^^^^^^^^



Actions
----------

``action:app.load``
^^^^^^^^^^^^^^^^^^^^^

**Argument(s)**: None

Executed when NodeBB is loaded, used to kickstart scripts in plugins (i.e. cron jobs, etc)

``action:page.load``
^^^^^^^^^^^^^^^^^^^^^

**Argument(s)**: An object containing the following properties:

* ``template`` - The template loaded
* ``url`` - Path to the page (relative to the site's base url)

``action:plugin.activate``
^^^^^^^^^^^^^^^^^^^^^

**Argument(s)**: A String containing the plugin's ``id`` (e.g. ``nodebb-plugin-markdown``)

Executed whenever a plugin is activated via the admin panel.

**Important**: Be sure to check the ``id`` that is sent in with this hook, otherwise your plugin will fire its registered hook method, even if your plugin was not the one that was activated.

``action:plugin.deactivate``
^^^^^^^^^^^^^^^^^^^^^

**Argument(s)**: A String containing the plugin's ``id`` (e.g. ``nodebb-plugin-markdown``)

Executed whenever a plugin is deactivated via the admin panel.

**Important**: Be sure to check the ``id`` that is sent in with this hook, otherwise your plugin will fire its registered hook method, even if your plugin was not the one that was deactivated.

``action:post.save``
^^^^^^^^^^^^^^^^^^^^^

**Argument(s)**: A post object (javascript Object)

Executed whenever a post is created or edited, after it is saved into the database.

``action:email.send``
^^^^^^^^^^^^^^^^^^^^^

``action:post.setField``
^^^^^^^^^^^^^^^^^^^^^

``action:topic.edit``
^^^^^^^^^^^^^^^^^^^^^

``action:post.edit``
^^^^^^^^^^^^^^^^^^^^^

``action:post.delete``
^^^^^^^^^^^^^^^^^^^^^

``action:post.restore``
^^^^^^^^^^^^^^^^^^^^^

``action:notification.pushed``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Argument(s)**: A notification object (javascript Object)

Executed whenever a notification is pushed to a user.

``action:config.set``
^^^^^^^^^^^^^^^^^^^^^

``action:topic.save``
^^^^^^^^^^^^^^^^^^^^^

``action:user.create``
^^^^^^^^^^^^^^^^^^^^^

``action:topic.delete``
^^^^^^^^^^^^^^^^^^^^^

``action:user.verify``
^^^^^^^^^^^^^^^^^^^^^
Parameters: uid; a hash of confirmation data (ex. confirm_link, confirm_code)
Useful for overriding the verification system. Currently if this hook is set, the email verification system is disabled outright.


``action:user.set``
^^^^^^^^^^^^^^^^^^^^^
Parameters: field (str), value, type ('set', 'increment', or 'decrement')
Useful for things like awarding badges or achievements after a user has reached some value (ex. 100 posts)

``action:settings.set``
^^^^^^^^^^^^^^^^^^^^^
Parameters: hash (str), object (obj)
Useful if your plugins want to cache settings instead of pulling from DB everytime a method is called. Listen to this and refresh accordingly.


Client Side Hooks
--------------------

``filter:categories.new_topic``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


``action:popstate``
^^^^^^^^^^^^^^^^^^^

``action:ajaxify.start``
^^^^^^^^^^^^^^^^^^^

``action:ajaxify.loadingTemplates``
^^^^^^^^^^^^^^^^^^^

``action:ajaxify.loadingData``
^^^^^^^^^^^^^^^^^^^

``action:ajaxify.contentLoaded``
^^^^^^^^^^^^^^^^^^^

``action:ajaxify.end``
^^^^^^^^^^^^^^^^^^^

``action:reconnected``
^^^^^^^^^^^^^^^^^^^

``action:connected``
^^^^^^^^^^^^^^^^^^^

``action:disconnected``
^^^^^^^^^^^^^^^^^^^

``action:categories.loading``
^^^^^^^^^^^^^^^^^^^

``action:categories.loaded``
^^^^^^^^^^^^^^^^^^^

``action:categories.new_topic.loaded``
^^^^^^^^^^^^^^^^^^^

``action:topic.loading``
^^^^^^^^^^^^^^^^^^^

``action:topic.loaded``
^^^^^^^^^^^^^^^^^^^

``action:composer.loaded``
^^^^^^^^^^^^^^^^^^^

``action:widgets.loaded``
^^^^^^^^^^^^^^^^^^^