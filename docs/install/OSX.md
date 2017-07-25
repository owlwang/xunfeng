# MAC OSX 安装指南

## 一、环境安装

### 1. 安装brew

使用 homebrew 在 Mac OSX 中进行软件的安装与管理, 执行如下命令安装 brew 工具:

```
$ sudo ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 2. 下载巡风

```
$ cd ~
$ brew install git
$ git clone https://github.com/ysrc/xunfeng.git
```

### 3. 操作系统依赖

安装系统依赖:

```
$ brew install gcc libffi libpcap openssl
```

### 4. python 依赖库

更新`pip`到最新版本:

```
$ sudo pip install -U pip
```

使用`pip`安装 python 依赖库, 这里使用了豆瓣的 pypi 源。

```
$ sudo pip install -r requirements.txt -i https://pypi.doubanio.com/simple/
```

### 5. 安装数据库

```
$ brew install mongodb
```

## 二、部署与配置

### 1. 启动数据库

```
$ sudo mongod --port 65521 --dbpath ~/xunfeng/db/ &
```
输入
```
$ ps aux | grep mongod
```
确定mongodb是否已启动，正常应有返回

### 2. mongodb 添加认证

```
$ sudo mongo 127.0.0.1:65521/xunfeng
> db.createUser({user:'username',pwd:'password',roles:[{role:'dbOwner',db:'xunfeng'}]})
> exit
```

这里的 `username`和`password` 分别为你想设置成的 mongodb 的用户名和密码。

### 3. 导入数据库

并且导入数据库:

```
$ sudo mongorestore -h 127.0.0.1 --port 65521 -d xunfeng ~/xunfeng/db
```

### 4. 修改配置

修改系统数据库配置脚本 `Config.py`,这里是系统的用户名和登录的密码:

```python
class Config(object):
    ACCOUNT = 'admin'
    PASSWORD = 'xunfeng321'
```

修改 `DBUSERNAME` 和 `DBPASSWORD` 字段内的用户名和密码, 设置成你在第2步设置的的 mongodb 的用户名密码。

```python
class ProductionConfig(Config):
    DB = '127.0.0.1'
    PORT = 65521
    DBUSERNAME = 'username'
    DBPASSWORD = 'password'
    DBNAME = 'xunfeng'
```
### 5. 运行系统

根据实际情况修改 `Conifg.py` 和 `Run.sh` 文件后, 执行:

```
$ sudo sh Run.sh
```
## 6. 进入巡风

打开浏览器，访问 http://127.0.0.1
输入用户名 admin 密码 xunfeng321 即可进入巡风。
