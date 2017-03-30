#接口调试解决bug方案
测试开发中最最最麻烦的就是调试bug,还是接口没有好的情况?不,我这个是接口已经对接好了,但之前的模拟数据还没学会,
所以这里并不是模拟接口,而是真正线上接口,但由于某种情况,你不能测试,接口测试关闭,或者是活动还没开启之类,
那么我只有2种方式来解决,这是其中之一,之二是使用软件客户端拦截,不太好用.实现方式`重定向请求`或`重写返回数据`

###重定向请求
如果你使用的是jq库的$.ajax来请求,那么还是可以拦截接口转向本地接口测试的,你可以使用以下方法进行拦截调整
```javascript
//使用jq自带的过滤器,可以重定向请求
	$.ajaxPrefilter( "json script", function( options, originalOptions, jqXHR ) {
		// Modify options, control originalOptions, store jqXHR, etc
		console.log(options,'///');
		console.log(originalOptions,'///');
		console.log(jqXHR,'///');

		if(!!options.url.match('test/test.json')){
            //重定向请求
			options.url = 'http://www.xxxx.com/activity/action.php?act=getServerList';
//			options.dataType = 'jsonp'; 
       }

	});
    //本地模拟数据接口
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

###返回数据重写
这个方法是我之前想到的,有个项目接口测试已经关闭,但有些数据返回也并不清楚,还有一种情况可以使用这种,就是线上项目出错
需要重写`返回数据`达到某种目的.为了不影响线上的功能,在url中添加一个`debug`参数为调试注入模式
还是拿上面的例子进行讲解
```javascript
/*
     test.json 数据
     {
     "ex": 0,
     "data": [
         {
            "id": "10009",
            "sn": "草泥马"
         },
         {
             "id": "10008",
            "sn": "草泥马"
         }
      ]
     }
	 */
    
	//用于重写的数据
	var resetDaet = {
		"ex": 1,
        "data":[
            {
	            "id": "10009",
	            "sn": "数据重写"
            },
            {
	            "id": "10008",
	            "sn": "数据重写2"
            }
        ]
	};

	//获取url的debug参数
	var debug = true; 

	
	$.ajax({

		url: 'http://192.168.202.188:9000/test/test.json',
		dataType: 'jsonp'
	}).done(function (data, status, jqXHR) {
		console.log('返回数据data:', data);
	});

	//必须建立一个同名 json_callback 函数
	function json_callback(d) {
		console.log('before:数据:', d); //照常理这里应该是未改变的,但现在不是,是什么原因?
		if (debug) {  //调试模式
			d = $.extend({}, d, resetDaet);
//          d = $.extend(d,resetDaet);  //不使用这个
		}
		console.log('after:数据:', d);
	}

```

这种就可以达到预留后门调试的效果.但好像如果调试的话,这个`resetDate`好像有边别扭,这里的数据可以在编写方法的时候预留跟调用方法一样的名字,再添加一个一样的变量名会好点.



