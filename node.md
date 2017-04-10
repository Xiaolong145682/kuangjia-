# Node.js 介绍
Node.js 不是一种独立的语言,与 PHP、Python、Perl、Ruby 的“既是语言也是平台” 不同。Node.js 也不是一个 JavaScript 框架,不同于 CakePHP、Django、
Rails。Node.js更不 是浏览器端的库,不能与 jQuery、ExtJS 相提并论。Node.js 是一个让 JavaScript 运行在服务 端的开发平台,它让 JavaScript 成为脚
本语言世界的一等公民,在服务端堪与 PHP、Python、 Perl、Ruby 平起平坐。
# Node.js 优点
1. 采用事件驱动、异步编程，为网络服务而设计。其实Javascript的匿名函数和闭包特性非常适合事件驱动、异步编程。
1. Node.js非阻塞模式的IO处理给Node.js带来在相对低系统资源耗用下的高性能与出众的负载能力，非常适合用作依赖其它IO资源的中间层服务。
1. Node.js轻量高效，可以认为是数据密集型分布式部署环境下的实时应用系统的完美解决方案。
# Node.js 缺点
1. 可靠性低  
2. 单进程，单线程，只支持单核CPU，不能充分的利用多核CPU服务器。一旦这个进程崩掉，那么整个web服务就崩掉了。  
# 缺点解决办法  
1. 开启多个进程，每个进程绑定不同的端口，用反向代理服务器如 Nginx 做负载均衡，好处是我们可以借助强大的 Nginx 做一些过滤检查之类的操作，同时能够实现比较好的均衡策略，但坏处也是显而易见——我们引入了一个间接层。
2. 多进程绑定在同一个端口侦听。在Node.js中，提供了进程间发送“文件句柄” 的功能，这个功能实在是太有用了（貌似是yahoo 的工程师提交的一个patch） ，不明真相的群众可以看这里： Unix socket magic
3. 一个进程负责监听、接收连接，然后把接收到的连接平均发送到子进程中去处理。在Node.js v0.5.10+ 中，内置了cluster 库，官方宣称直接支持多进程运行方式。Node.js 官方为了让API 接口傻瓜化，用了一些比较tricky的方法，代码也比较绕。这种多进程的方式，不可避免的要牵涉到进程通信、进程管理之类的东西。

## node.js安装
#### 安装nodejs的方法
```js
$ curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```
#### 开发环境的安装
```js
$ sudo apt-get install -y build-essential
```
## npm使用
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：  
```
* 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
* 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
* 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。
```
## npm第三方库的安装
#### 本地安装
```
$ npm install express        # 本地安装
```
#### 全局安装
```
$ npm install express -g     # 全局安装
```
#### 强制重新安装
`$ npm install <packageName> --force`
#### 更新已安装的模块
`$ npm update <packageName>`
## nvm使用
`定义:node版本管理器`
### 安装nvm
`$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash`
#### 查看远程node.js版本信息
`$ nvm ls-remote `
#### 查看本地所有node.js版本信息
`$ nvm ls`
