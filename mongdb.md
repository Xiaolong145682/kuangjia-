## 安装mongodb
1. 添加公共keyserver
```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
2. Create a list file for MongoDB.(添加下载地址)
```
cd /etc/apt/
$ echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
3. 升级软件包
```
sudo apt-get update
```
4. Install the latest stable version of MongoDB.
```
sudo apt-get install -y mongodb-org
```
## 启动数据库
1. 启动mongodb.service
```
service mongod start
```
2. 启动数据库
```
mongo
//或者
mongo 127.0.0.1:27017
```
