# centos7.5部署node应用



## 安装nvm

[原教程地址：云服务器安装nvm管理node](http://www.geekjc.com/post/5cb82908a9381e261825a155)

1. 安装nvm
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`
// 或者
wget -qO- <https://raw.githubusercontent.com/creationix/nvm/v0.35.3/install.sh> | bash
```

```
// 使用nvm 命令之前执行以下命令
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

建议使用 nvm 安装 Node，因为 nvm 会安装到用户的目录，而 n 会安装到全局的 /usr/ 目录下去。

2. 安装 Node
```bash
nvm install 12.18.3
// 如果安装了多版本，则可以将默认的 Node 版本设置成 8.9.3：
nvm alias default 8.9.3
```



## 安装mysql

1. 先检查系统是否装有[mysql](https://cloud.tencent.com/product/cdb?from=10680)

```bash
rpm -qa | grep mysql
```

（这里执行安装命令是无效的，因为centos-7默认是Mariadb，所以执行以下命令只是更新Mariadb数据库 yum install mysql 删除 yum remove mysql）

2. 下载mysql的repo源

```bash
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```

3. 安装mysql-community-release-el7-5.noarch.rpm包

```
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

4. 安装mysql

```
sudo yum install mysql-server
```

> 这里可能会报错 请注意

```javascript
Error: Package: mysql-community-libs-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libc.so.6(GLIBC_2.17)(64bit)
Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libc.so.6(GLIBC_2.17)(64bit)
Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: systemd
Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libstdc++.so.6(GLIBCXX_3.4.15)(64bit)
Error: Package: mysql-community-client-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libc.so.6(GLIBC_2.17)(64bit)
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
```

> 解决方法如下：

```
yum install glibc.i686 yum list libstdc++*
```

5. 然后登陆 没有密码直接回车跳过 mysql -u root -p

>  登录时有可能报这样的错：ERROR 2002 (HY000): Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock’ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：

```bash
sudo chown -R root:root /var/lib/mysql
```

6. 重启：

```
service mysql restart
```

7. 密码设置

```javascript
mysql > use mysql;
mysql > update user set password=password('123456') where user='root';
mysql > exit;
```

8. 再重启 登陆mysql  加上远程访问权限 为root添加远程连接的能力。链接密码为 “root”（不包括双引号）

```javascript
mysql> GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "root";　　
```

9. 查询不区分大小写表名

```bash
vim /etc/my.cnf

# 在[mysqld]节点下，加入一行： lower_case_table_names=1
# 重启
```



## 用到的linux命令

- 上传文件到服务器

  ```
  scp <本地文件地址> root@服务器地址:要放到的路径
  ```

- 在服务器上解压缩zip文件

  ```
  # 安装 zip unzip
  yum install zip unzip
  
  # 解压
  unzip 文件名
  ```

  

## mysql表

```mysql
# posts
CREATE TABLE `posts` (
  `id` int(5) unsigned zerofill NOT NULL AUTO_INCREMENT,
  `contentmd` varchar(10000) DEFAULT NULL,
  `contenthtml` varchar(10000) DEFAULT NULL,
  `time` datetime DEFAULT NULL COMMENT '发布时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=146 DEFAULT CHARSET=utf8

# admin
CREATE TABLE `admin` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `token` varchar(500) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8
```

