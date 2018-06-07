```在Ubuntu18.04中安装mysql5.7失败```

## 报错代码

```bash
正在设置 mysql-server-5.7 (5.7.22-0ubuntu18.04.1) ...
Renaming removed key_buffer and myisam-recover options (if present)
Unable to determine if daemon is running: No such file or directory
Warning: Unable to start the server. Please restart MySQL and run mysql_upgrade to ensure the database is ready for use.
Unable to determine if daemon is running: No such file or directory
Warning: Unable to start the server.
Job for mysql.service failed because the control process exited with error code.
See "systemctl status mysql.service" and "journalctl -xe" for details.
invoke-rc.d: initscript mysql, action "start" failed.
● mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
   Active: activating (auto-restart) (Result: exit-code) since Thu 2018-06-07 11:13:28 CST; 4ms ago
  Process: 2572 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.pid (code=exited, status=1/FAILURE)
  Process: 2563 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
dpkg: 处理软件包 mysql-server-5.7 (--configure)时出错：
 installed mysql-server-5.7 package post-installation script subprocess returned error exit status 1
dpkg: 依赖关系问题使得 mysql-server 的配置工作不能继续：
 mysql-server 依赖于 mysql-server-5.7；然而：
  软件包 mysql-server-5.7 尚未配置。

dpkg: 处理软件包 mysql-server (--configure)时出错：
 依赖关系问题 - 仍未被配置
因为错误消息指示这是由于上一个问题导致的错误，没有写入 apport 报告。
                                                                    在处理时有错误发生：
 mysql-server-5.7
 mysql-server
E: Sub-process /usr/bin/dpkg returned an error code (1)

```

## 错误原因

```因为之前安装了mysql8.0```，在删除mysql8.0的时候没有彻底删除。

## 解决方法

### 彻底删除mysql

```bash
# 利用apt彻底删除mysql
sudo apt-get autoremove --purge mysql-server

# 清理残留数据
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```

### 彻底删除后，重新安装mysql
