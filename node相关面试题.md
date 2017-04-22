## node概览
* 为什么要用node?

参考答案：总结起来node有以下几个特点：简单强大，轻量可扩展．简单体现在node使用的是javascript,json来进行编码，人人都会；强大体现在非阻塞ＩＯ，可以适应分块传输数据，较慢的网络环境，尤其擅长
高并发访问；轻量体现在node本省即是代码，又是服务器，前后端使用统一语言；可扩展体现在可以轻松应对多实例，多服务器构架，同时有海量的第三方应用组件．
* node的构架是什么样子的？

参考答案：主要分为三层，应用层app＞＞V8及node内置架构＞＞操作系统．`V8是node的运行环境，可以理解为node虚拟机`．node内置架构又可分为三层：核心模块（javascript实现）＞＞
c＋＋绑定＞＞libuv + CAes + http.

![node架构图](http://joaopsilva.github.io/talks/End-to-End-JavaScript-with-the-MEAN-Stack/img/nodejs-arch-ppt.png)
* node有哪些核心模块？
参考答案：EventEmitter,Stream,FS,Net和全局对象
## 全局对象
1. node有哪些全局对象？

参考答案：process,console,Buffer和exports

2. process有哪些常用方法？

参考答案： process.stdin, process.stdout, process.stderr, process.on, process.env, process.argv, process.arch, process.platform, process.exit

3. console有哪些常用方法?

参考答案: console.log/console.info, console.error/console.warning, console.time/console.timeEnd, console.trace, console.table

4. node有哪些定时功能?

参考答案: setTimeout/clearTimeout, setInterval/clearInterval, setImmediate/clearImmediate, process.nextTick

5. node中的事件循环是什么样子的?

总体上执行顺序是：process.nextTick >> setImmidate >> setTimeout/SetInterval

看官网吧：
[官网](https://github.com/nodejs/node/blob/master/doc/topics/event-loop-timers-and-nexttick.md)

6. node中的Buffer如何应用?

参考答案: Buffer是用来处理二进制数据的，比如图片，mp3,数据库文件等.Buffer支持各种编码解码，二进制字符串互转．

