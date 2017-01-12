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

 ####变量作用域
 支持函数的嵌套，变量是从内向外搜索，内部函数可以使用外部函数的变量，但是外部函数不能使用内部函数的变量。
 函数会对未定义的变量进行一个变量提升，而不会报错。
 全局作用域的变量被绑定到了window这个变量上。比如alter可以写成window.alter()
 不同的js文件中拥有相同的全局变量的话，会引起冲突。可以将所有的变量绑定到一个全局变量中：
    var MYAPP = {};
    MYAPP.name = 'name';
    MYAPP.version = 1.0;
    
    MYAPP.foo = function(){
      return 'foo';
    };

js的变量作用域是在函数内部的，在块级作用域中，可以使用let来定义变量，比如在for中可以使用 for(let i =0; i< 100;i++)
ES6 引入了const来定义常量，他和let都是块级别作用域。

#### 方法
绑定到对象上的函数称为方法，和普通函数没差，但是可以使用一个this变量
this变量始终指向当前的对象。
如果带有this的函数单独调用，一般会得不到正确结果，必须用xx.xx()才可以。
如果遇到了嵌套定义的函数，那么可以在最外层使用var that =this 获取到当前的对象
函数本身提供了一个apply函数，可以用来指定this指向哪个对象，apply接受两个参数，第一个是需要绑定的this变量，第二个参数是Array，表示函数本身的参数。
    function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
    }
    
    var xiaoming = {
    name: '小明',
    birth: 1990,
    age: getAge
    };
    
    xiaoming.age(); // 25
    getAge.apply(xiaoming, []); // 25, this指向xiaoming, 参数为空

另外一个相似的函数 call(), 只是call的参数是顺序传入的，而apply的第二个参数是打包成array传进去的。

对普通函数的调用，通常把this绑定为null。
    Math.max.apply(null,[3,4,5]);

装饰器，利用apply来动态的修改函数的行为。比如想统计使用了多少次parseInt()，可以将原来的函数替换掉。

    var count = 0;
    var oldParseInt = parseInt;
    
    window.parseInt = function(){
      count+=1;
      return oldParseInt.apply(null,arguments);
    }
这相当是在原来的parseInt上加了一个修饰。结果并没有变化。

####高阶函数
map、reduce
map是用来映射的。reduce一般接受两个参数。
[x1,x2,x3,x4].reduce(f) = f(f(f(x1,x2),x3),x4)
map一般会对function传入三个参数， value, index ,array

filter和map一样也接收一个函数，filter将函数依次作用于元素，根据返回值是true还是false来决定是否保留还是丢弃这个元素。
    var arr = [1,2,3,4,5,6]
    var r = arr.filter(function(x){
      return x % 2 !== 0;
    });
    r; //[1,3,5]

####闭包
函数可以作为函数的返回，这个在使用redurce和map这类高阶函数中使用比较多。 也可以实现出一个lazy的感觉，在调用函数的时候才会执行函数。
     function lazy_sum(arr) {
     var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
      }
       return sum;
    }

返回闭包函数时不能在返回函数中引入任何的循环变量，或者是后续会发生变化的变量。

闭包可以创建一个私有变量

闭包同样可以将多参数函数变成单参数函数，比如pow(x,y)

    function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
       }
    }
    
    // 创建两个新函数:
    var pow2 = make_pow(2);
    var pow3 = make_pow(3);
    
    pow2(5); // 25
    pow3(7); // 343

####箭头函数

```javascript
x => x *x 

(x,y) => x+y

() => 3.14

(x,y, ...rest) => {

  var i,sum = x + y;

  for(i = 0; i< rest.length;i++){

 sum+= rest[i];

  }

  return sum

}
```

返回一个对象的话，需要将对象用()包围起来。 x => ({foo:x})
箭头函数的this总是指向了词法作用域，即外层调用者obj。
在使用call和apply的时候则无法对this进行绑定，也就是说apply和call的第一个参数没什么效果。

```javascript
var obj = {
    birth: 1990,
    getAge: function (year) {
        var b = this.birth; // 1990
        var fn = (y) => y - this.birth; // this.birth仍是1990
        return fn.call({birth:2000}, year);
    }
};
obj.getAge(2015); // 25
```

#### generator
生成器的函数可以用function* foo(x){....} 定义， 其中使用yield来返回需要的值。
使用generator的时候有两种方法。 使用next方法，一个是使用for..of
比如：
    var f = foo(5);
    f.next(); //结果是{value:0, done: false}
一种：
    for(var x of fib(5)){
      console.log(x); //输出结果
    }
注意next返回的是一个对象，里面有两个属性，当时done属性变成true的时候，则不再继续执行了。表示全部执行完成。

#### 标准对象
不要使用new Number()、new Boolean()、new String()创建包装对象；
用parseInt()或parseFloat()来转换任意类型到number；
用String()来转换任意类型到string，或者直接调用某个对象的toString()方法；
通常不必把任意类型转换为boolean再判断，因为可以直接写if (myVar) {...}；
typeof操作符可以判断出number、boolean、string、function和undefined；
判断Array要使用Array.isArray(arr)；
判断null请使用myVar === null；
判断某个全局变量是否存在用typeof window.myVar === 'undefined'；
函数内部判断某个变量是否存在用typeof myVar === 'undefined'。

number的对象转换成string的话，可以使用123..toString(); 或者(123).toString()

#### 正则表达式
RegExp来创建一个正则表达式：
    var re1 = /ABC\-001/;
    var re2 = new RegExp('ABC\\-001');
这有两种方式来创建正则表达式。

RegExp提供了test的方法，用来检测给定的字符串是否符合条件。
切分字符串：
'a b   c'.split(/\s+/); // ['a','b','c']
用正则表达式切分的更加的彻底。

分组，正则表达式有一个()的功能，将匹配到的内容作为一个组。在js中如果在正则表达式中加入了(),则在使用exec的时候回得到匹配的子串。
    var re = /^(\d{3})-(\d{3,8})$/;
    re.exec('010-12345'); // ['010-12345', '010', '12345']
    re.exec('010 12345'); // null

贪婪匹配，默认的正则是贪婪匹配的，如果要采用非贪婪匹配，可以在后面加一个？

    var re = /^(\d+?)(0*)$/;
    re.exec('102300'); //['102300','1023','00'];
    
全局搜索 在正则后面加g
    var r1 = /\d+/g

#### Json
JSON.stringify(object,null,' ')
第一个是对象，第二个参数可以使要筛选的对象的键值。可以传入array，也可以传入函数。函数的参数可以传入convert(key,value)

如果想精确控制object，可以给object定义一个toJSON的方法。比如：
    var a {
      name: "小明"，
      age: true;
      toJSON: function(){
        return {
          'Name':this.name,
          'Age': this.age
        };
      }
    };
这样经过json的序列化后，返回的只有name和age。

反序列化，可以使用JSON.parse(), 可以接受一个函数作为参数，参数同样是key，value



#### 原型继承
js中没有办法直接继承，需要修改原型链来达到继承的目的，可以使用一个通用的继承函数：
    function inherits(Child, Parent) {
        var F = function () {};
        F.prototype = Parent.prototype;
        Child.prototype = new F();
        Child.prototype.constructor = Child;
    }
  
  使用的时候，主要有如下三步：
  - 定义新的构造函数，并在内部用call()调用希望“继承”的构造函数，并绑定this；
  - 借助中间函数F实现原型链继承，最好通过封装的inherits函数完成；
  - 继续在新的构造函数的原型上定义新方法
 
 比如：
 	function PrimaryStudent(props) {
    	Student.call(this, props); //使用call来调用
    	this.grade = props.grade || 1;
	}

#### class继承
上面的那些继承方法，到了ES6中进行了很大的简化，其中出现了class的定义。我们可以使用class来定义一个新的类：
	class Student{
      constrictor(name){
        this.name = name;
      }
      
      hello(){
        console.log("hello, "+ this.name + '!');
      }
	}
	
	var xm = new Student("xiaoming");
	xm.hello();
	
继承的话就更加简单了：
	class PrimaryStduent extends Student{
      constructor(name,grade){
        super(name); //继承父类的属性
        this.grade = grade;
      }
      
      say() {
        console.log("I am ",+this.name);
      }
	}

不过这样的代码目前并不是所有的浏览器都支持，因此可以使用babel来进行转换。

#### 浏览器对象
window的innerWidth和innerHeight属性表示内部宽度。同样也有outerwidth和outerHeight

- navigator.appName：浏览器名称；
- navigator.appVersion：浏览器版本；
- navigator.language：浏览器设置的语言；
- navigator.platform：操作系统类型；
- navigator.userAgent：浏览器设定的User-Agent字符串

screen表示屏幕信息，有width，height，colorDepth
location表示当前的页面的URL信息 protocol,host,port,pathname.search hash等
重新加载一个新页面location.assign() 重新加载location.reload()

document表示当前页面，是整个页面的DOM的根节点
document.title 获取页面的title
document的getElementByID()和getElementByTagName()可以获取一个tag或者一组DOM节点。
同样document可以获取cookie

#### 操作DOM
getElementById('#id') 使用id名获取到对应的节点
getElementsByClassName('xxx')  返回一组节点
getElementsByTagName('xx') 返回具体Tag的节点

返回的一组元素，可以用下标进行访问。 同时可以使用.children获取当前节点下所有的子节点。
firstElementChild
lastElementChild 
这两个是表示当前节点的第一个或者是最后一个

更加高效的是是用querySelector()和querySelectorAll(),不过语法略有不同。

#### 更新DOM
可以修改innerHTML来修改标签内部的内容,这可以任意改变标签内部的内容。
innerText 会自动化对html进行编码，
textContent也是这样的，不过前者不返回隐藏元素文本，后者是都返回

修改style属性的css，可以用 p.style.color = "#ff0000"; 
注意在CSS中的font-size的命名，到了js中需要使用驼峰的命名方式 fontSize。

#### 插入DOM
可以使用appendChild();
如果当前插入的节点存在于当前的文档树，这个节点首先会从原先的位置删除，在插入到新的位置。

	var 
	   js = document.getElementById('js'),
	   list = document.getElementById('list');
	list.appendChild(js)；

也可以从零开始创建一个节点：

	nodes = document.createElement('p'); //创建一个p节点
	nodes.id = 'haskell';
	nodes.innerText = 'Haskell'; //<p id="haskell">Haskell</p>
	xxx.appendChild(nodes);
	
可以使用 setAttribute('xxx', "xxx")

insertBefore(new,ref) 将new插入到ref节点的前面

#### 删除DOM
removeChild(child) 需要获取到父节点， 删除后父节点下的子节点数量就会动态更新，所以在使用循环删除的时候需要注意使用下标的问题。

删除后的节点不在document中，但是还在内存中。

#### 操作表单
获取到表单的id，然后可以使用value/check这两个属性


