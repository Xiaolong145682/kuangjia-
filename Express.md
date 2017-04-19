## Express框架
Express.js是基于Node.js中HTTP模块和Connect组件的web框架，这些组件叫做中间件，它们是以约定大于配置原则作为开发的基础理念．
`中间件（middleware`：就是处理HTTP请求的函数。它最大的特点就是，一个中间件处理完，再传递给下一个中间件。` `实例在运行过程中，会调用一系列的中间件。
## Express　安装
本地：
```
$ npm i express --save
```
全局：
```
$ npm i express-generator -g
```
## 创建模块
```
npm init
npm install 第三方库　--save //本地安装
```
## 路由器
#### 定义：启动脚本web.js的app.get方法，用于指定不同的访问路径所对应的回调函数，这叫做“路由”（routing）
* 实现一个动态网页的代码如下：
```js
var express = require("express");
var app = express();
app.get('/',function(req,res){
    res.send("Hello world!");
});
app.listen(3000);
```
* 终端上运行：
```vim
node web.js
```
* 网页显示：`Hello world!`　　
* 多个路由:
```js
var express = require('express');
var app = express();

app.get('/', function(req, res){
  res.send("Hello world!");
});

app.get('/doc', function(req, res){
  res.send('doc page');
});

app.get('/admin', function(req, res){
  res.send('admin page');
});

app.listen(3000);
```
#### 多路由管理：把路由放到一个单独的`index.js`文件中, 比如新建一个routes子目录．
* 引用管理后的路由:
```js
var express = require('express');
var app = express();
var routes = require('./routes/index.js')(app);
app.listen(3000);
```
#### 基于字符串模式的路由路径匹配实例
1. 此路由路径将匹配 acd 和 abcd。
```js
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});
```
2. 此路由路径将匹配 abcd、abbcd、abbbcd 等。
```js
app.get('/ab+cd', function(req, res) {
  res.send('ab+cd');
});
```
3. 此路由路径将匹配 abcd、abxcd、abRABDOMcd、ab123cd 等。
```js
app.get('/ab*cd', function(req, res) {
  res.send('ab*cd');
});
```
4. 此路由路径将匹配 /abe 和 /abcde。
```js
app.get('/ab(cd)?e', function(req, res) {
 res.send('ab(cd)?e');
});
```
5. 也可通过正则表达式去匹配．
## 响应方法
下表中响应对象 (res) 的方法可以向客户机发送响应，并终止请求/响应循环。如果没有从路由处理程序调用其中任何方法，客户机请求将保持挂起状态。
```js
res.download()	  提示将要下载文件。
res.end()	        结束响应进程。
res.json()	      发送 JSON 响应。
res.jsonp()	      在 JSONP 的支持下发送 JSON 响应。
res.redirect()	  重定向请求。
res.render()	    呈现视图模板。
res.send()	      发送各种类型的响应。
res.sendFile	    以八位元流形式发送文件。
res.sendStatus()	设置响应状态码并以响应主体形式发送其字符串表示。
```
## 模板引擎用于express
### 安装模板引擎
```
$ npm install ejs --save
$ npm install jade --save
```
在 Express 可以呈现模板文件之前，必须设置以下应用程序设置：
```
views：模板文件所在目录。例如：app.set(‘views’, ‘./views’)
view engine：要使用的模板引擎。例如：app.set(‘view engine’, ‘ejs’)
```
改用render方法，对网页模板进行渲染:
```js
var express = require('express');
var app = express();

app.set('view engine', 'ejs');
app.set('views', __dirname+'/views');


app.get('/', function (req, res){
  //
	res.render('index', {name: 'express'});
});

app.get('/about', function(req, res) {
	res.render('about');
});

app.get('/article', function(req, res) {
	res.render('article');
});

app.listen(3000);
```
重点解释：`render`方法的参数就是模板的文件名，默认放在子目录views之中，后缀名已经在前面指定为html，这里可以省略。所以，res.render(‘index’) 就是指，把子目录views下面的index.html文件，交给模板引擎ejs渲染。
## ejs模块引擎
注意到任何在<% %>之间的代码都被执行了，而在<%= %>标签内的都把这自己返回的HTML字符串插入到了当前位置里。 我们需要添加JavaScript代码来控制模板的载入的渲染。 我们将用下面的代码来替换原来的字符串代码:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>EJS Demo</title>
</head>
<body>
  <h1>Weclome to <%= name %></h1>

</body>
</html>
```
使用jinclude可以引用文件模板
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>EJS Demo</title>
</head>
<body>
  <div>
    <% include ./header %>
  </div>

  <div>
    <% include ./footer %>
  </div>

</body>
</html>
```
`index.ejs`
```html
<div>
  <h1>欢迎你使用EJS模板引擎!</h1>
</div>
```
`header.ejs`  
## forEach的使用
如果一条数据, 我们可以随意的传输, 但如果是多条数据怎么办? 在这里 EJS 提供了一个 forEach 来解决一组数据的处理问题. 下面的我们就模拟一组 json 数据:
```js
app.get('/about', function(req, res) {
  var info = [{name: 'Mary', age: 20},
  {name: 'Ben', age: 32},
  {name: 'Scotch', age: 21}
];
	res.render('about', {
    info: info,
    title: "Information"
  });
});
```
下面是我们的 HTML 文件, 这里面用到的就是 forEach 来处理一组数据的显示.
```js
<div>
  <h2> <%= title %> </h2>
  <ul>
    <% info.forEach(function(info){ %>
      <li><strong>Name:</strong><%= info.name %>  --- <strong>age:</strong><%= info.age %></li>
    <% }); %>
  </ul>
</div>
```
## GET/POST获取数据
### GET使用
* 使用GET提交数据时, 我们需要使用req.query来进行获取.
1. 前端使用get方法(默认方法)提交信息.
```
<form class="aaa" action="/about">
    <input type="text" name="name" value="">
    <button type="submit" name="button">button</button>
</form>
```
2. 提交完成后, app.js 文件后台使用req.query.name来进行获取.
 ```js
 app.get('/about', function(req, res) {
  var info = [{name: 'Mary', age: 20},
  {name: 'Ben', age: 32},
  {name: 'Scotch', age: 21}
];
console.log(req.query.name);
	res.render('about', {
    info: info,
    title: "Information"
  });
});
 ```
 ### POST使用
 在post请求时, 我们需要获取提交的数据时, 需要安装一个 bodyParser的包.
 ```
 $ npm install body-parser --save
 ```
安装完后需要在app.js文件中增加下面的两项设置.
```js
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

// 指定模板文件的后缀名为ejs

app.set('view engine', 'ejs');
app.set('views', __dirname+'/tpl');

app.use(bodyParser.json({limit: '1mb'}));  //body-parser 解析json格式数据
app.use(bodyParser.urlencoded({            //此项必须在 bodyParser.json 下面,为参数编码
  extended: true
}));

app.get('/', function (req, res){
	res.render('index', {name: 'express'});
});

app.post('/about', function(req, res) {
  var info = [{name: 'Mary', age: 20},
  {name: 'Ben', age: 32},
  {name: 'Scotch', age: 21}
];
console.log(req.body);
	res.render('about', {
    info: info,
    title: "Information"
  });
});

app.listen(3008);
```
将前端提交的数据经过处理后变成json数据格式. 所以,我们可以直接 req.body.name , req.body.age 来获取前端提交的数据进行处理.
```js
app.post('/about', function(req, res) {
  var info = [{name: 'Mary', age: 20},
  {name: 'Ben', age: 32},
  {name: 'Scotch', age: 21}
];
console.log(req.body.name);
console.log(req.body.age);
	res.render('about', {
    info: info,
    title: "Information"
  });
});
```
