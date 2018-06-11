---
layout: post
title: JS跨域问题
comments: true
toc: true
date: 2017-10-19 10:14:16
tags:
	- Java
	- JavaScript跨域
	- Jsonp
---

JavaScript不能再不同的主机名、端口和协议之间进行数据传输或者通讯, 此时称为JS跨域, 解决方法有很多, 这里介绍下使用Jsonp的方式.

<!--more-->

## <font color=orange> 跨域解决方法 </font>

### <font color=orange> Jsonp </font>
JavaScript不能跨域访问, 但是可以引入不同域上面的js文件, Jsonp正是利用此特性来实现的.Jsonp返回的是一个JS文件, 返回的JS文件的时候会立即执行回调函数, 并且会把数据作为函数参数.

Jsonp需要前后端进行配合.

例如: 
xxx.html中跨域请求
```
#普通写法
<script>
    function hangler(jsondata) {
        //code
    }
</script>
<script src="http://example.com/login/callback=handler" />

#JQuery其他写法, 默认参数名称callback
$.ajax({
    url : "http://example.com/login",
    dataType : "jsonp",
    type : "GET",
    success : function(data){
        //code
    }
});
```

callback是回调的参数名, 也可以是其他的名称, handle是回调的函数名, 当请求完成时回执行该方法, 使用jQuery时`$.getJSON`方法会自动判断是否跨域，不跨域的话，就调用普通的ajax方法；跨域的话，则会以异步加载js文件的形式来调用jsonp的回调函数。

后台: 
```
//指定请求路径、方法以及返回值类型
@RequestMapping(value = "/token/{token}", method = RequestMethod.GET,produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
//返回JSON
@ResponseBody
//@PathVariable从请求地址中绑定数据
//callback为Jsonp请求时的函数名
public String getUserByToken(@PathVariable String token, String callback) {
    Result result = userService.getUserByToken(token);
    //Jsonp请求返回js文件
    if (StringUtils.isNoneBlank(callback)) {
        return callback + "(" + JsonUtils.objectToJson(result) + ")";
    }
    return JsonUtils.objectToJson(result);
}
```

Spring4.1版本以后还可以:
```
@RequestMapping(value = "/token/{token}", method = RequestMethod.GET)
@ResponseBody
public Object getUserByToken(@PathVariable String token, String callback) {
    Result result = userService.getUserByToken(token);
    if (StringUtils.isNoneBlank(callback)) {
        MappingJacksonValue value = new MappingJacksonValue(result);
        //设置回调方法
        value.setJsonpFunction(callback);
        return value;
    }
    return result;
}
```


