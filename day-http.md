## http结构
HTTP协议构建在请求和响应的概念上，对应在node.js中就是由http.ServerRequset和http.ServerResponse这两个构造器构造出来的对象．web服务器随后会给出响应．
```js 
var http = require('http');
var serv = http.createServer(function(req, res){
    res.writeHead(200);
    res.end(' Welcome to Node.js!');
}).listen(3000);
```
在终端上建立一个telnet连接，并发送请求：
```
$ telnet 127.0.0.1 3000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
GET / HTTP/1.1

HTTP/1.1 200 OK
Date: Tue, 15 Nov 2016 01:09:18 GMT
Connection: keep-alive
Transfer-Encoding: chunked

14
 Welcome to Node.js!
0
```
##　头信息
HTTP协议和IRC一样流行，其目的是进行文档交换．它在请求和响应消息前使用头消息来描述不同的消息内容，web页面回发生许多不同类型的内容：文件text,HTML,XML,JSON,以及JPEG
图片等．
*　具体代码
```js
var http = require('http');
var serv = http.createServer(function(req, res){
    res.writeHead(200, { 'Content-Type': 'text/html' });//规定服务器发送过来的内容是什么类型
    res.end('<h1>Welcome to Node.js!</h1>');
}).listen(3000);
```
## 一个简单的web服务器
#### 不多说看代码
```js
var http = require('http');
var serv = http.createServer(function(req, res){
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(['<h1>Welcome to Node.js!</h1>',
    '<form method="POST" action="/url">',
    '<fieldset>',
    '<label> Personal information </label>',
    '<p>Name</p>',
    '<input type="text" name="name">',
    '<p><button>Submit</button></p>',
    '</form>'
].join(' '));
}).listen(3000);
```
#### method和url
修改上面的代码, 展示一下提交表单后, 我们需要处理的表单.
```js
var http = require('http');
var serv = http.createServer(function(req, res){
    //判断页面是否打开了
    if( '/' == req.url){
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end(['<h1>Welcome to Node.js!</h1>',
    '<form method="POST" action="/url">',
    '<fieldset>',
    '<label> Personal information </label>',
    '<p>Name</p>',
    '<input type="text" name="name">',
    '<p><button>Submit</button></p>',
    '</form>'
].join(' '));
} 
//当点击了提交按钮，提交了信息
else if ('/url' == req.url){
    res.writeHead(200, {'Content-Type': 'text/html' });
    res.end('Your sent a <em>' + req.method + '</em> request');
}
}).listen(3000);
```
