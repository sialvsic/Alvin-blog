---
title: Mysql install uninstall and simple use
date: 2016-08-28 20:39:03
tags:
- mysql
---

# Mysql install uninstall and simple use

------------------

## MAC下安装
### 推荐使用homebrew安装
执行下列安装命令

更新brew
```
brew  update
```
安装mysql
```
brew install mysql
```
安装完之后会出现提示：
```
We've installed your MySQL database without a root password. To secure it run: `mysql_secure_installation`
To connect run: `mysql -u root` To have launchd start mysql at login:
ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
Then to load mysql now:
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
Or, if you don't want/need launchctl, you can just run:

mysql.server start
```

`此处仅为了解即可，建议不要执行以下该命令`
然后执行**mysql_secure_installation** 会出现

```
> Securing the MySQL server deployment.
>
Connecting to MySQL using a blank password.
>
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?
>
Press y|Y for Yes, any other key for No: No
Please set the password for root here.
>
New password:  Vaystar@0917
>
Re-enter new password:
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.
>
Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.
>
>
Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.
>
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : N
 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.
>
>
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 -Dropping test database...
Success.
>
 -Removing privileges on test database...
Success.
>
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.
>
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.
>
All done!

到此安装完毕，使用时输入mysql -u root -p 的命令即可，输入密码
我的密码为:  安装于`2016.8.26`

Vaystar@0917
```

### 出现的问题
#### CLI下输入MySQL command not found
问题分析：环境变量设置不对
解决方法：
在.zshrc 中写入，之后source一下即可。

```
export PATH=$PATH:/usr/local/mysql/bin
```

### 自己尝试的错误
开始在官网上下载dmg 的安装包，正常安装后出现下列问题
http://stackoverflow.com/questions/33326065/unable-to-access-mysql-after-it-automatically-generated-a-temporary-password
mysql会自动的生成一个random password，但你在使用**mysql -u root -p** 后输入该随机的密码时又会出错，在网上找了各种方法无果。
所以，

>  mac下安装不推荐使用dmg安装

遂之后使用  brew install mysql 安装，开始有些冲突，最后莫名其妙的好了，也是醉了。

## MAC下卸载

1.关闭mysql服务

```
mysql.server stop
```

2.依次执行以下命令

```
sudo rm /usr/local/mysql   
sudo rm -rf /usr/local/mysql*
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
rm -rf ~/Library/PreferencePanes/My*
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
sudo rm -rf /private/var/db/receipts/*mysql*
sudo rm -rf /var/db/receipts/com.mysql.
调用vim /etc/hostconfig，删除 MYSQLCOM=-YES-这一行。
```

3.在根目录下执行以下命令 查找依旧存在的mysql  并删除 千万别乱删，容易出事
```
 find . mysql | grep mysql
```

可以删除
- /usr/local/var/mysql  整个文件
- /usr/local/share/mysql
- /usr/local/Cellar/mysql  整个文件


`Note:` 可能会发现以上的命令在执行时根本不存在或者没有该文件，是因为mysql 的版本不同导致。

参考链接

1.stackoverflow：http://stackoverflow.com/questions/1436425/how-do-you-uninstall-mysql-from-mac-os-x

2.http://www.jb51.net/article/81447.htm

日志：
2016.8.28

使用brew install mysql  ，但是手贱执行mysql_secure_installation，并设置了中级难度的root密码，结果发现在跑黑工项目时，默认的root密码是空，我就GG了，其实改一下项目的配置文件就好了，但是强迫症 + 完美主义患者是不喜欢凑合的，所以研究了一下如何卸载重装mysql

## How to start run and stop

Usage: mysql.server  {start|stop|restart|reload|force-reload|status}  [ MySQL server options ]
> mysql.server start

> mysql.server stop

> mysql.server restart

> mysql.server status
