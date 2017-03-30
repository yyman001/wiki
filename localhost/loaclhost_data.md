#本地接口数据模拟方案
为了测试对接调试接口,使用了`mock2easy`,本地服务器的,如果是使用jq的ajax的,可以使用拦截改变url方式继续调试
本来想使用mock.js 来的,但好像jsonp不能拦截,这样就没有意义了,而且跟`browserSync`使用有冲突,现在大多数测试环境都是调用`jsonp`吧,少数调用`json`
建议先跟后端沟通规定好接口相关`url`和`返回数据`

[mock2easy](https://www.npmjs.com/package/mock2easy) 具体可以看官方的参数

1.安装`mock2easy`  
```cmd
//安装到你需要的地方，本人使用的是gulp,配合gulp使用
npm install mock2easy --save-dev
```
2.配置`mock2easy`的`gulp`任务
```javascript
///////////////////////模拟数据请求系统//////////////////////
gulp.task('serve', function () {
 
  // 在这配置mockeasy的配置
  var options = {
    port:3100, 	         //端口号
    lazyLoadTime: 3000,  //接口延迟时间
    database: 'mock2easy', //数据库[默认]
    doc: 'doc',			 //文档[默认]
    ignoreField: [],	 //?
    interfaceSuffix: '.json', //文件后缀[默认]
    preferredLanguage: 'cn'   // en => 英文版 , cn => 中文版
  };
 
 // 启动mock2easy,注意demo例子里端口号为3000，请避开端口重复 
  require('mock2easy')(options, function (app) {
    try {
      app.listen(options.port, function () {
        console.log(('mock2easy is starting , please visit : http://localhost:' + options.port).bold.cyan);
      });
    } catch (e) {
      console.log(e);
    }
  });
 
  //这个为接口服务器地址[ajax请求的服务器地址]
  browserSync({
    notify: false,
    port: 9000,  //端口
    server: {
      baseDir: ['.tmp', 'app'], //?
      routes: {
        '/bower_components': 'bower_components'
      },
      middleware: [require('./mock2easy/do')] //注意这里来定义middleware ，如果不配置的话将不能对接口进行拦截
    }
  });

});
///////////////////////模拟数据请求系统//////////////////////
```

3.运行`gulp serve`任务,会有个黑色窗口监听服务器的

4.在浏览器输入接口页面(以本例子)

http://localhost:3100/#/

在右上角`添加新接口`添加接口`/test/test.json`,点击确定会自动跳到设置页面,`注意:本地jsonp方式,需要新建一个跟jsonp同名的函数作为跨域调用`

有`返回动态接口`和`返回静态接口`,我这里选择的是`返回静态接口`,直接把需要返回的`json数据`填进去即可

```javascript
$.ajax({
		url: 'http://localhost:9000/test/test.json',
		dataType: 'jsonp'
	}).done(function (data, status, jqXHR) {
        //这里不会被触发
		console.log('返回数据data:', data);
	});
	//必须建立一个同名 json_callback 函数
    function json_callback(d) {
    	    //触发跨域函数
		    console.log(d);
	    }
```
到这里就已经可以使用本地的模拟数据了
