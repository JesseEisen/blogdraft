#### 读文件

有两种方式来读取文件，一个是异步读，一个是同步读。
	var fs = require('fs')
	fs.readFile('sample.txt','utf-8', function(err,data){
      if(err){
        console.log(err);
      }else{
        console.log(data);
      }
	});
	
同步读取：
	var data = fs.readFileSync('sample.txt','utf-8');
	console.log(data);
同步读取和C语言的读取是一样的，如果要判断是否有错误，可以用try……catch

读二进制文件的时，回调函数返回一个Buffer对象。不过这个Buffer可以和string互换，比如：
	var text = data.toString('utf-8');
	console.log(text);
	
或者用Buffer将string转成buffer
	var buf = new Buffer(text,'utf-8');