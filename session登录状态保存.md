## 登录状态保存之express-session
1. session的定义：session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而session保存在服务器上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上，这就是session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。
2. session与cookie保存登录状态的区别：
    * cookie数据存放在客户的浏览器上，session数据放在服务器上;
    * cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗 考虑到安全应当使用session;
    * session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能 考虑到减轻服务器性能方面，应当使用COOKIE;
    * 单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie.
### 具体案例
1. 安装第三方模块express-session
```
npm install express-session --save
```
2. 在node框架下的app.js文件中应用第三方库
```js
var express = require('express');
var session = require("express-session");

var app = express();
app.use(session({
  resave: true,
  saveUninitialized: false,
  secret: '3nqr9xzx2438fgsdam4324n',//通过设置的 secret 字符串，来计算 hash 值并放在 cookie 中，使产生的 signedCookie 防篡改
  cookie: {
      maxAge: 1000 * 60 * 30 //设置存放时间，时间到了就自动清除cookie
  }
}));

```
3. 具体的使用
```js
var express = require('express');
var router = express.Router();
var users = require("../db/users");
var bcrypt = require("bcrypt");
var salt = 10;

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { email: '',passwd: ""});
});

router.post("/denglu",function(req,res,next){
  var data2 = req.body;
  // console.log(data2);
  users.find({
    email: data2.email,
  }, function(err, doc){
    // console.log(doc);
    if (err) {
      console.log(err);
    } else if (doc.length !== 0){
       bcrypt.compare(data2.passwd, doc[0].password, function(err, hash){
        //  console.log(hash);
          if (hash) {
            req.session.email = doc[0].email;
             return res.redirect("/dengluok");
          } else {
            return res.render("index",{email:"", passwd: "密码输入不正确!"});
          }
       })
    } else {

       return res.render("index",{email:"该账户不存在", passwd: ""});

    }

  });

});

router.get("/dengluok",function(req,res){
  users.findOne({email:req.session.email},function(err,doc){
    if(err){
      console.log(err);
    }
    if(typeof(doc) === "undefined"){
      req.session.email = null;
    }
    res.render("denglu",{title:req.session.email});
  })
});

router.get("/tuichu",function(req,res){
  console.log("+++",req.session.email);
  req.session.email = null;
  res.render("denglu",{title:req.session.email});
})

module.exports = router;

```
### session(options)
#### session(options)方法是express-session的主要方法，其中options中包含可选参数，主要有：
* name: 设置 cookie 中，保存 session 的字段名称，默认为 connect.sid ;
* store: session 的存储方式，默认存放在内存中，也可以使用 redis，mongodb 等。express 生态中都有相应模块的支持;
* secret: 通过设置的 secret 字符串，来计算 hash 值并放在 cookie 中，使产生的 signedCookie 防篡改;
* cookie: 设置存放 session id 的 cookie 的相关选项，默认为 (default: { path: ‘/’, httpOnly: true, secure: false, maxAge: null });
* genid: 产生一个新的 session_id 时，所使用的函数， 默认使用 uid2 这个 npm 包;
* rolling: 每个请求都重新设置一个 cookie，默认为 false;
* resave: 即使 session 没有被修改，也保存 session 值，默认为 true。


