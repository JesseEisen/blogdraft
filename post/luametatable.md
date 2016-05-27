> 本文翻译自:[Lua Metatables Tutorial](http://nova-fusion.com/2011/06/30/lua-metatables-tutorial/)

我将会在这篇教程中讲到在Lua中一个非常重要的概念：`metatable` . 知道如何使用metatable后会让你更好的使用Lua。每个table都可以有一个metatable。matetable是通过某些key来改变它所绑定的table的一些行为的。让我们来看一个例子:

```
t = {} --正常定义的一个table
mt = {} -- metatable， 不过现在没有包含任何东西
setmetatable(t,mt) --设置mt成为t的metatable
getmetatable(t) -- 这将返回mt
```

我们可以发现：`getmetatable` 和`setmetatable`是两个主要的函数，很明显从字面就可以看出他们是做什么的。当然，我们可以将上面的前三行写成如下的样子：

```
t = setmetatable({},{})
```

`setmetatable` 返回它的第一个参数，所以我们可以用这样的惯用法。

我们可以在metatable中放些什么呢？metatable中可以放任何的东西，但是他们需要和以`__`(两个下划线)开头的key对应。比如:`__index` 和`__newindex`. 这些keys对应的值通常是函数或者是其他的表。如下：

```lua
t = setmetatable({},{
  __index = function(t, key)
  	if key == "foo" then
  		return 0
  	else
  		return table[key]
  	end
  end
})
```

上面我们将一个函数赋值给了`__index`现在让我们来看看这个关键字到底是关于什么的。

## __index

使用的最多的metatable的key就是`__index`. 它可以包含其他的函数和表。

当我们用一个key(table是key-value形式)在表中搜索是，不管这个key是什么(t[4],t.foo或者t["foo"]).当值没有赋给这个key的时候，Lua会在table的metatable中寻找`__index`, 如果`__index`包含在这个表中，Lua将在属于`__index`的表中寻找对应的`Key`. 这也许看起来比较的绕，让我们看一个例子：

```
other = {foo = 3}
t = setmetatable({}, {__index = other})
t.foo   -- 3
t.bar   -- nil
```

如果`__index`包含一个函数，那这个函数将会被调用。这个函数的参数是我们用于遍历的表以及我们要搜寻的`key`(译注：这边的key是指table的key，而不是指metatable的关键字).我们从上面的倒数第二个例子可以看出，在这个`__index`关键字内我们可以使用条件判断，以及任何Lua代码能够做到的。因此，在那个例子中，如果key等于"foo",我们就返回`0`,否则我们就用那个key去搜寻`table`表。我们用`t`作为`table`的一个别名，如果这个key`foo`被使用就返回0.

你也许会疑惑为什么table会作为`__index`函数的第一个参数，这很好解释，如果你在多个表中使用这个metatable，他支持了代码的复用以及节约了一定的电脑资源。我们将在`Vector`类中像你展示这些。

## __newindex

下一个是`__newindex`, 这个和`__index`类似。和`__index`一样，他可以包含函数或者其他的表。

当你尝试去给一表设置不存在的键值得时候，Lua会搜索是否`__newindex`关键字存在于这个metatable中，它的步骤和`__index`一样。如果`__newindex`是一个表，那么这个key和value将会被设置在`__newindex`所指定的表中。

```
other = {}
t = setmetatable({},{__newindex = other})
t.foo = 3
other.foo   -- 3
t.foo  -- nil
```

如果`__newindex`是一个函数，那么这个函数将被调用，key和value将作为参数传入

```
t = setmetatable({}, {
	__newindex = function(t,key,value)
		if type(value) == "number" then
			rawset(t,key, value * value)
		else
			rawset(t,kye,value)
		end
	end
})

t.foo = "foo"
t.bar = 4
t.la = 10
t.foo  -- "foo"
t.bar -- 16
t.la  -- 100
```

当在t中创建一个新的key的时候，如果这个value是一个number，那他将被平方，否则他都会被设置。
同时这个例子也引出了`rawget` 和`rawset`

## rawget 和 rawset

他们是在我们需要设置和获取表的key的时候，不想让Lua将metatable引进来。正如你猜的那样，`rawget`允许你获取这个`key`所代表的值，同时让Lua不是使用`__index`. `rawset`让你设置这个`key`的value，同时不会去使用`__newindex`(这些并不会对速度上有神提升，如果使用常规做法的话)。当你陷入无限循环的时候你可能需要使用他们。举个例子，上面的那个代码例子中，`t[key] = value * value` 将会在`__newindex`中设置一遍，这将会让你陷入无线循环中。使用`rawset(t,key, value * value)`会避免这个。