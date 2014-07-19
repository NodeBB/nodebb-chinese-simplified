NodeBB 编码规范
==================

大多数情况下，NodeBB 与 `Google Javascript 编码规范 <http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml>`_ 一致。

代码格式化
-------------------

.. note::
	
	2013年7月前存在的代码并不 100% 遵守此规范。如果你看到不符合规范的代码，请按规范格式化，然后提交推送请求。

缩进、括号
-------------------

NodeBB 使用制表符缩进。括号依照 `One True Brace Style <http://en.wikipedia.org/wiki/Indent_style#Variant:_1TBS>`_:

.. code:: javascript

    if (condition) {
        // code here ...
    } else {
        // otherwise ...
    }

条件和语句分列不同行，即使已有一条语句也要使用花括号括起来：

.. code:: javascript

    if (leTired) {
        haveANap();
    }

错误
-------------------

大多数回调函数第一个参数返回错误。先处理错误，然后再执行逻辑。

.. code:: javascript

    someFunction(parameters, function(err, data) {
        if(err) {
           return callback(err); // or handle error
        }
        // proceed as usual
    });

变量
-------------------

变量始终使用 `var` 关键字定义：

.. code:: javascript

    var foo = 'bar';

多个定义包含在同一个 `var` 语句中：

.. code:: javascript

    var foo = 'bar',
        bar = 'baz';

分号
-------------------

所有可用分号的地方都要使用分号

命名
-------------------

所有的地方都使用驼峰法命名：

.. code:: javascript

	functionNamesLikeThis, variableNamesLikeThis, ClassNamesLikeThis, EnumNamesLikeThis, methodNamesLikeThis, CONSTANT_VALUES_LIKE_THIS, foo.namespaceNamesLikeThis.bar, and filenameslikethis.js.
