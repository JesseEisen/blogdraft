#### 变量
使用'use strict' 限制定义变量使用var

#### array
修改array的length属性可以动态改变array的长度，但不推荐使用
下标的索引值超过了长度，会自动增加array的长度
indexOf()搜索一个指定元素的位置
slice()可以用来截取Array的部分元素，可以有如下的方式：
  arr.slice(0,3)
  arr.slice(3)  从索引3到最后 
slice()不提供参数的话，则是复制整个数组。
push() 向尾部添加若干元素， pop()把最后一个元素删掉
unshift往头部添加若干元素，shift则是删掉第一个元素
sort对array排序，直接修改当前的array
reverse 反转array
splice()是一个比较万能的方法
  arr.splice(2,3,"Google","Facebook")  从索引2开始删掉3个元素，添加两个元素
  arr.splice(2,2) 只删除不添加
  arr.splice(2,0,"Google") 只添加不删除
concat()把当前的array和另一个array连接起来，并返回一个新的array,但是并没有修改当前的array
   var added = arr.concat([1,2,3])
join() 方法将array的每个元素连接起来，可以指定一个连接符
   var arr = ['A','B','C',1,2,3];
   arr.join('-'); //'A-B-C-1-2-3'

#### 对象
在访问对象的属性的时候，可以使用['xxx'] 也可以使用object.prop，如果是包含了特殊的字符的属性名，必须使用['xxx']来访问。
可以通过delete来删除一个对象的属性 delete object.prop
使用in来判断某个对象是否拥有某一个属性，这个属性可能是继承过来的
使用hasOwnProperty来判断是不是自身的属性
   object.hasOwnProperty('name')

#### 条件判断
if() {...} else {...}  
if() {...} else if() {...} else{...}

js将null undefined 0 NaN和空字符''视为false，别的都是true。

#### 循环控制
for()基本用法是和c语言的一样，for(express1;express2;express3){...}
for ... in  一般是有如下的语法：
    for(var key in o){
      alter(key); 
    }
也可以对array使用for in，但是此时返回的索引是string类型
while() 和 do ...while()和c语言中的一样的。

####map和set
这两个是ES6新加的
js中object的属性只能是字符串，这样显然不是很全面的，因此可以使用Map来进行一个关联，这个时候会生成一个key-value的键值对，这个键值对可以使number是键
基本的使用：
  var m = new Map()
  m.set(key,value)
  m.get(key)
  m.delete(key)

Set是一组key的集合，但不存储value，在set中key不能重复。基本使用：
   var s = new Set([1,2,3]);
   s.add(4)
   s.delete(3)

#### iterable
array可以用下标访问，但是map和set不可以，ES6引入了iterable的概念，这仨都是属于iterable类。

可以通过 for .... of 来循环遍历。注意返回的是值，而不是索引值，对于Map而言是返回键值对的组合。
对于array而言，for of 可以将不属于array的属性排除在外。
使用iterable的最直接的方式是forEach方法，接受一个函数，每次迭代的时候都会执行这个函数：
    var a = ['a','b','c'];
    a.forEach(function(element,index,array){
    alert(element);
    });
函数里面的三个参数，名称不固定，但是位置是固定的，第一个参数是值，第二个是索引值，第三个是本身。
对于Set而言，是比较特殊的，他没有索引，所以第二个参数是sameElement相关的。或者说和第一个参数是一样的。
用不到的参数可以直接省略

####函数
函数的定义 
    function name(variable){ ... }
    var name = function(variable){...} //匿名函数

传入到函数的参数数量不受影响。
如果要判断函数的参数，可以使用arguments来判断，比如arguments.length表示传入的参数的个数。同时arguments里面保存的就是参数的列表
当定义的参数没有传入的参数多的时候，想获取除了定义了的参数外的其余参数，可以使用rest来代替，这是ES6里面引入的。
    function foo(a,b, ...rest){
      ....
    }
 这个和c语言里面的可变参数比较类似。
 
 




