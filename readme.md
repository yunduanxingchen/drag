/*
* dev11
* created by chenchen  on 2016/10/08
* 应用程序的启动(入口)文件
* */

var express = require("express");

//创建app应用
var app = express();
//加载模版处理模块
var swig = require("swig");
//加载数据库模块
var mongoose = require('mongoose');
// 配置应用模版
//  定义当前应用所使用的模版引擎
// 第一个参数:模版引擎的名称,同时也是模版文件的后缀,第二个参数表示用于解析模版内容的方法
app.engine('html',swig.renderFile);
// 设置模版文件存放的目录,第一个参数必须是views,第二个参数是目录
app.set('views','./views');
//注册所使用的模版引擎,第一个参数必须是view engine,第二个参数和app.engine这个方法中定义的模版引擎的名称(第一个参数)是一致的
app.set('view engine','html');
//在开发过程中,需要取消模版缓存
swig.setDefaults({cache:false});


// 根据不同的功能划分模块,
app.use('/admin',require('./routers/admin'));
app.use('/api',require('./routers/api'));
app.use('/',require('./routers/main'));
//监听http请求



mongoose.connect('mongodb://localhost/blog',function(err){
    if(err){
        console.log("数据库连接失败")
    }else{
        console.log("数据库连接成功");
        app.listen(8081);
    }
});





//设置静态文件托管目录
//当用户访问的url以public开始,那么直接返回对应__dirname+'/public'下的文件
// app.use("/public",express.static(__dirname + '/public'));


//用户发送http请求, -》 url -》 解析路由 -》 找到匹配的规则 -》 执行绑定的函数,返回对应内容

//public -> 静态 -》 直接读取指定目录下的文件,返回给用户 -》 动态 -》 处理业务逻辑 -》 返回数据给用户