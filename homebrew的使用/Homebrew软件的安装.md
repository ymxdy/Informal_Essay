# sa eHomebrew软件的安装

[TOC]

## jdk的安装与环境变量的配置

### 安装

+ 搜索软件

```shell
brew  search adoptopenjdk
```

+ 安装软件
  + 安装源（我使用的是第二个）
    + homebrew/cask-versions/adoptopenjdk8
    +  adoptopenjdk/openjdk/adoptopenjdk8

```shell
brew install --cask adoptopenjdk/openjdk/adoptopenjdk8
```

+ 安装完毕后，输入下面的指令

```shell
# 查看java版本号
java -version
# 回显版本号的相关信息
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_292-b10)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.292-b10, mixed mode)
```

### 环境变量的配置

+ 查看jdk安装地址

```shell
# 查看地址
/usr/libexec/java
# 回显
Matching Java Virtual Machines (1):
    1.8.0_292 (x86_64) "AdoptOpenJDK" - "AdoptOpenJDK 8" /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
```
+ 从终端输出结果可以得出：JAVA_HOME路径

```shell
/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
```

+ 查看是否配置了环境变量

```shell
echo $JAVA_HOME
如果出现空白，说明还没有配置
```
+ 配置环境变量（在.zshrc添加下面几行）

```shell
export JAVA_HOME=$(/usr/libexec/java_home)
export PATH=$JAVA_HOME/bin:$PATH
export CLASS_PATH=$JAVA_HOME/lib
```
+ 使修改的文件生效
```shell
source ~/.zshrc
```

+ 查看环境变量是否生效

```shell
echo $JAVA_HOME
如果出现空白，说明还没有生效

# 如果能正确输出Java安装目录，则说明配置已生效
/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
```

## maven的安装
+ brew查找maven版本
```shell
brew search maven
```
+ 安装
```shell
brew install maven@3.5
```
+ maven在zsh环境变量配置，安装完了有提示的，直接复制就行
```shell
echo 'export PATH="/usr/local/opt/maven@3.5/bin:$PATH"' >> ~/.zshrc
```
+ 查看是否安装成功

```shell
# 查看是否安装成功
mvn -v
# 回显
Maven home: /usr/local/Cellar/maven@3.5/3.5.4_1/libexec
Java version: 1.8.0_292, vendor: AdoptOpenJDK, runtime: /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/jre
Default locale: zh_CN, platform encoding: UTF-8
OS name: "mac os x", version: "10.16", arch: "x86_64", family: "mac"
```

## git的安装

+ brew 查找git版本

```shell
brew search git
```

+ brew 安装Git

```shell
brew install git
```

+ 安装完毕后查看版本号

```shell
# 查看git的版本号
git --verison
# 回显
git version 2.33.0
```

## node的安装

+ brew 查找node版本

```shell
brew search node
```

+ brew 安装node

```shell
brew install node@12
```

+ node在zsh环境变量配置，安装完了有提示的，直接复制就行

```shell
echo 'export PATH="/usr/local/opt/node@12/bin:$PATH"' >> ~/.zshrc
```

+ 配置npm

> 遇到过 cnpm 的 bug，所以不适用 cnpm，直接配置淘宝镜像			

```shell
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global

# 查看镜像源是否配置成功
npm config get registry
npm config get disturl
```

## mysql的安装


+ brew查找mysql版本
```shell
brew search mysql
```
+ 安装
```shell
brew install mysql@5.7
```
+ 开机自动启动
```shell
brew services start mysql@5.7

# 如果你不想/不需要后台服务，你可以运行:
/usr/local/opt/mysql@5.7/bin/mysqld_safe --datadir=/usr/local/var/mysql
```
+ mysql在zsh环境变量配置，安装完了有提示的，直接复制就行
```shell
export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"
```
+  连接mysql，root用户无初始密码，直接回车
```shell
mysql -uroot -p

ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)  ---这个情况是mysql还没有被启动
```
+  设置root用户初始密码，为'root'
```mysql
set password for 'root'@'localhost' = 'root';
```

+ mysql创建新的用户（必须在root用户下执行）

```mysql
// 语句
CREATE USER '用户名'@'ip' IDENTIFIED BY '名称';
// 样列
CREATE USER 'wangfeng'@'LOCALHOST' IDENTIFIED BY 'wangfeng';
```

+ mysql新用户赋予权限（必须在root用户下执行）

```mysql
grant 权限 on *.* to '用户'@'ip'; //  *.* 代表所有库下的所有表

// 比如给该用户下所有的库赋予查询的权限
grant select on *.* to '用户'@'ip';

// 给用户赋予crud的权限
grant select,delete,update,create,drop on *.* to 'wangfeng'@'localhost' identified by "wangfeng";

// 赋予所有的权限
grant all privileges on *.* to '用户'@'ip';

赋权限后记得刷新： flush privileges;

```
+ mysql 删除权限

```mysql
// 查看用户的权限
show grants for 'wangfeng'@'localhost';

// 删除用户的权限
revoke all privileges on *.* from '用户'@'ip';

// 刷新权限
flush privileges;

```



####  删除用户

  + 要查看MySQL服务器中的数据库:mysql的所有用户信息

  ```mysql
  USE mysql;
  SELECT user, host FROM mysql.user;
  ```

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h3r6amzvloj20zw0u0whe.jpg" alt="截屏2022-07-01 09.30.41" style="zoom:50%;" />

 + 假设您要删除用户帐户：`wangfeng`，请使用以下语句：

  ```mysql
  DROP USER 'wangfeng';
  再次从mysql.user表中查询数据，您将看到用户api@localhost已被删除。
  
  // 要删除单个DROP USER语句中两个用户：remote_user和auditor的帐户，请使用以下语句：
  DROP USER user1, user2;
  ```

