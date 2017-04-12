# 文件系统FS
`fs`模块是文件操作的封装，它提供了文件的读取、写入、更改、删除、目录、链接等`POSIX`文件系统操作。与其他模块不同的是，`fs`模块中所有的操作都提供了异步和同步的两个版本，
例如读取文件的函数又异步的`fs.readFile()`和同步的`fs.readFileSync()`。
### fs.readFile
`fs.readFile(filename,[enconding],[callback(err,data)])`是最简的读取文件的函数。
* 参数介绍：
    * filename:表示要读取的内容；
    * enconding:可选参数，表示文件的字符编码;
    * callback:回调函数，它提供的两个参数err和data,err表示有错误发生，data表示文件内容。若指定了enconding，data是一个解析后的字符串，否则data将会是以Buffer形式表示的二进制数据。
* 例子：
```js
var fs = require('fs');

fs.readFile('./content.md', function(err, data){
  if(err){
    console.log(err);
  }else {
    console.log(data);
  }
});
```
### fs.readFileSync
`fs.readFileSync(filename, [encoding])`是 fs.readFile 同步的版本。它接受的参数和 fs.readFile 相同，而读取到的文件内容会以函数返回值的形式返回。如果有错误发生，fs 将会抛出异常，你需要使用 try 和 catch 捕捉并处理异常。
* 例子：
```js
try {
  var data = fs.readFileSync('./file1.md', 'utf-8');
  console.log(data);
} catch (err) {
  console.log(err);
}
```
### writeFile()
`writeFile`方法用于异步写入文件。  
* fs.writeFile(filename, data,[encoding], [callback(err)])
    * 第一个参数是要写入的文件名；
    * 第二个参数是需要写入的数据;
    * 前两个是必选参数 , 第三个参数是编码, 默认是utf8;
    * 第四个参数是回调函数.
* 例子：
```js
var fs = require('fs');
var data = "The is write file."
fs.writeFile('./abcd.md', data, function(err){
  if(err){
    console.log(err);
  }
});
```
### writeFileSync
`fs.writeFileSync(filename, data, [encoding])`,该方法用于同步写入文件。
* 例子：
```js
var fs = require('fs');
var data = "the write file."

try {
  fs.writeFileSync('./aaa.md', data, 'utf-8');
} catch (err) {
  console.log(err);
}
```
### exists
* exists方法用来判断给定路径是否存在，然后不管结果如何，都会调用回调函数。
* 例子：
```js
var fs = require('fs');
var util = require('util');

fs.exists('./node', function(exists){
  util.debug(exists ? "it's there" : "No dir.")
})
```
## 创建删除目录
### 创建目录
1. `fs.mkdir(path, [mode], [callback(err)])`
* 参数介绍：
      * path:表示目录名；
      * mode:可选参数，表示权限值；
      * callback:可选参数，表示回调函数；
### 删除目录
1. `fs.rmdir(path, [callback(err)])`  
2. 综合例子
* 例子：检查当前目录下, 是否存在tem目录, 如果存在将它删除, 如果不存在将创建它.
```js
var fs = require('fs');
var util = require('util');
var tem = './file';
//检查该文件是否存在
 fs.exists(tem,function(exists){
  if(exists){
  //存在
    console.log("It's here " + tem + "\nI will delete it." );
    fs.rmdir(tem,function(err){
      if(err){
        console.error("删除失败！");
      }else{
        console.log('删除成功！');
      }
    });
  }
  //不存在
  else{
    fs.mkdir(tem,function(err){
      console.log("No " + tem + "\nI will create it.");
      if(err){
        console.error("创建失败");
      }else{
        console.log('创建file成功！');
      }
    });
  }
});

```
### stat
* 获取文件`fs.stat(path, [callback(err, stats)])`
* 方法介绍：`stat`方法的参数是一个文件或目录，它同够回调函数放回一个stats对象，该对象包含了该文件或目录的具体信息。我们往往通过该方法，判断正在处理的到底是一个文件，还是一个目录。
### read
1. 打开文件 fs.open(path,flags, [mode], [callback(err, fd)])
2. 关闭文件 fs.close(fd, [callback(err)])
3. 读取文件(文件述符) fs.read(fd,buffer,offset,length,position, [callback(err, bytesRead, buffer)])
4. 写入文件(文件述符) fs.write(fd,buffer,offset,length,position, [callback(err, bytesWritten, buffer)])
* 参数介绍：
      * fd: 文件描述符
      * buffer: 读或写的缓冲区
      * offset: 开始读或写的偏移量
      * length: 读或写的字节数.
      *opsition: 指定了从哪里开始读取文件，如果position为null，将会从文件当前的位置读取数据。回调函数给定了三个参数， (err, bytesRead, buffer)， 分别为错误，读取的字节和缓冲区。
      


