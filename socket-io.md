## socket.io
socket.io是一个为实时应用提供跨平台实时通信的库．scoket.io旨在使实时应用应用到每个浏览器和移动设备上成为可能，模糊不同传输机制之间的差异．　　
socket.io是一个以实现浏览器，跨平台的实时应用为目的的项目，针对不同的浏览器版本或者不同的客户端会做自动降级处理，选择合适的实现方式（websocket,long pull ..）,隐藏
实现只暴露统一接口．可以让应用只关注于业务层面上．
## 安装
安装命令：
```js
npm install socket.io --save
```
现在我们在 express 使用 socket.io. 创建一个叫 app.js 的文件.
```js
var app = require('express')();
var server = require('http').Server(app);
//将 socket.io 绑定到服务器上，于是任何连接到该服务器的客户端都具备了实时通信功能
var io = require('socket.io')(server);
server.listen(3000);
app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

//作用是服务器监听所有客户端，并返回该新连接对象
io.on('connection', function(socket){
  console.log("a user connected!");
  socket.emit('news', {hello: 'world'});
  socket.on('other', function(data){
    console.log(data);
  });
});
```
前端页面代码：
```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Demo socket.io</title>
  <script src="http://cdn.bootcss.com/socket.io/1.5.1/socket.io.min.js"></script>
  <script>
  var socket = io();
  socket.on('news', function(data){
    console.log("socket.on");
    console.log(data);
    socket.emit('other', { my: 'data'});
  });
  </script>
</head>
<body>
<h1>Welcome socket.io.</h1>

</body>
</html>
```
## 重点
不管是服务器还是客户端都有 emit 和 on 这两个函数，可以说 socket.io 的核心就是这两个函数了，通过 emit 和 on 可以轻松地实现服务器与客户端之间的双向通信。

* `emit`：用来发射一个事件或者说触发一个事件，第一个参数为事件名，第二个参数为要发送的数据，第三个参数为回调函数（一般省略，如需对方接受到信息后立即得到确认时，则需要用到回调函数）。

* `on`：用来监听一个 emit 发射的事件，第一个参数为要监听的事件名，第二个参数为一个匿名函数用来接收对方发来的数据，该匿名函数的第一个参数为接收的数据，若有第二个参数，则为要返回的函数。
* `socket.io`:提供了三种默认的事件（客户端和服务器都有）：connect 、message 、disconnect 。当与对方建立连接后自动触发 connect 事件，当收到对方发来的数据后触发 message 事件（通常为 socket.send() 触发），当对方关闭连接后触发 disconnect 事件。
socket.io 还支持自定义事件，毕竟以上三种事件应用范围有限，正是通过这些自定义的事件才实现了丰富多彩的通信。
* 在服务器端区分以下三种情况：
    * socket.emit()：向建立该连接的客户端广播;
    * socket.broadcast.emit() ：向除去建立该连接的客户端的所有客户端广播;
    * io.sockets.emit() ：向所有客户端广播，等同于上面两个的和
