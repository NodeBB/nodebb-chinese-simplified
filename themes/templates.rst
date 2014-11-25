渲染引擎
=================

工作原理
------------------------------------------------------

每个页面都有相关的 API 调用、模板文件和语言文件。

For example, if you navigate to `/topic/351/nodebb-wiki <http://community.nodebb.org/topic/351/nodebb-wiki>`_, the application will load three resources. The API return `/api/topic/351/nodebb-wiki <http://community.nodebb.org/api/topic/351/nodebb-wiki>`_ and the `template <http://community.nodebb.org/templates/topic.tpl>`_, in this example, "topic.tpl", and the appropriate `language file <community.nodebb.org/language/en_GB/topic.json>`_ "topic.json"*.

Just prepend api/ to the URL's path name to discover the JSON return. Any value in that return can be utilized in your template.

*A page's name corresponds to the template and language's filename (ex. ``http://domain.com/topic/xyz`` correlates to ``topic.tpl``). Sometimes this is not the case - ex. ``/user/xyz`` loads ``account.tpl``. Have a look at the ``custom_mapping`` section in ``public/templates/config.json`` for more details.

模板基础
------------------------------------------------------

Using the API return as your guide, you can utilize any of those values in your template/logic. Using the above API call as an example, for anything in the root level of the return you can do something like:

.. code:: html

    {topic_name}

To access values in objects:

.. code:: html

    {privileges.read}

And finally you can loop through arrays and create blocks like so:

.. code:: html

	<!-- BEGIN posts -->
	{posts.content}
	<!-- END posts -->


The above will create X copies of the above block, for each item in the posts array.

模板逻辑
------------------------------------------------------

NodeBB 的模板系统实现了一些基础的逻辑。Using the same API call as above for our example. 可以像下面这样写条件判断(IF):

.. code:: html

	<!-- IF unreplied -->
	此帖不可回复！
	<!-- ENDIF unreplied -->


另一个例子:

.. code:: html

	<!-- IF !disableSocialButtons -->
	<button>分享到 Facebook</button>
	<!-- ELSE -->
	分享已被禁用。
	<!-- ENDIF !disableSocialButtons -->


可以检查数组的长度，如下:

.. code:: html

	<!-- IF posts.length -->
	 此处有回帖
	<!-- ENDIF posts.length -->


While looping through an array, we can check if our current index is the @first or @last like so:

.. code:: html

	<!-- BEGIN posts -->
	  <!-- IF @first -->
	    <h1>Main Author: {posts.username}</h1>
	  <!-- ENDIF @first -->
	  {posts.content}
	  <!-- IF @last -->
	    End of posts. Click here to scroll to the top.
	  <!-- ENDIF @last -->
	<!-- END posts -->


For more advanced documentation, have a look at the `templates.js <https://github.com/psychobunny/templates.js>`_ repository


传递模板参数给客户端 JavaScript
------------------------------------------------------

有两种方式，让 JS 接收到客户端的数据, 除了 WebSockets 之外 (TODO: 将会在其他文章中叙述)。

通过 jQuery.get
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If we require data from a different page we can make a ``$.get`` call to any other API call. For example, if we wanted to know more about a specific user we could make a call like so:

.. code:: javascript

	$.get(RELATIVE_PATH + '/api/user/psychobunny', {}, function(user) {
	    console.log(user)
	});


See this API call in action: http://community.nodebb.org/api/user/psychobunny

通过模板变量
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In topic.tpl for example, we can add a hidden input like so:

.. code:: html

    <input type="hidden" template-variable="pageCount" value="{pageCount}" />

The template system will immediately parse all of these and expose them via the following method:

.. code:: html

    ajaxify.variables.get('pageCount');

This is the ideal method of letting JS know about important variables within the template.

国际化
---------------------

The template engine interfaces with the internationalization system as well. We can embed variables into language strings. Let's use `this API call <http://community.nodebb.org/api/register>`_ as well as this `language file <http://community.nodebb.org/language/en_GB/register.json>`_ as an example. We can now do something like the following:

.. code:: html

    [[register:help.username_restrictions, {minimumUsernameLength}, {maximumUsernameLength}]]

Which will translate this string:

.. code:: html

    A unique username between %1 and %2 characters

to

.. code:: html

    A unique username between 2 and 16 characters

高级话题
---------------------

Dynamically requiring and rendering a template file from client-side JavaScript
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The template engine lazy loads templates on an as-needed basis and caches them. If your code requires a template or partial on-demand then you can  :

.. code:: javascript

	ajaxify.loadTemplate('myTemplate', function(myTemplate) {
		var html = templates.parse(myTemplate, myData);
	});


You can also access the invidual blocks inside each template, which is handy for doing things like (for example) rendering a new post's ``<li>`` and dynamically sticking it in an already loaded ``<ul>``

.. code:: html

	Some stuff here...
	<!-- BEGIN posts -->
	We just want to pull this block only.
	<!-- END posts -->
	... some stuff here

.. code:: javascript

	ajaxify.loadTemplate('myTemplate', function(myTemplate) {
		var block = templates.getBlock(myTemplate, 'posts');
		var html = templates.parse(block, myData);
	});


在服务端 Node.js 中渲染模板
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The templating system hooks into Express just like most other templating frameworks. Just use either ``app.render`` or ``res.render`` to parse the appropriate template.

.. code:: javascript

	res.render('myTemplate', myData);

.. code:: javascript

	app.render('myTemplate', myData, function(err, parsedTemplate) {
		console.log(parsedTemplate);
	});
