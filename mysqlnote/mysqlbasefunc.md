# 记录mysql的基本操作

## 登录
```bash
[root@host]# mysql -u root -p
Enter password:******
```

## 对database的操作
**显示所有database**
```bash
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| newYangshan        |
| performance_schema |
| tst_db             |
| yangshan           |
+--------------------+
6 rows in set (0.02 sec)
```

**进入database**
```base
mysql> use newYangshan;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

**查看database中的表格**
```bash
mysql> show tables;
+-----------------------+
| Tables_in_newYangshan |
+-----------------------+
| EMPLOYEE              |
| REPORT                |
| WARN_PAR              |
| portWeather           |
| sendTo                |
| shipStatic            |
| weatherData           |
+-----------------------+
7 rows in set (0.00 sec)

```

## 对table的操作
**查看table的字段描述**
```bash
mysql> desc REPORT;
+----------+-----------+------+-----+---------+-------+
| Field    | Type      | Null | Key | Default | Extra |
+----------+-----------+------+-----+---------+-------+
| pubDate  | int(11)   | NO   | PRI | NULL    |       |
| pubClock | int(11)   | NO   | PRI | NULL    |       |
| report   | text      | YES  |     | NULL    |       |
| njd      | int(11)   | YES  |     | NULL    |       |
| srcLoc   | char(20)  | NO   | PRI |         |       |
| srcUrl   | char(200) | YES  |     | NULL    |       |
+----------+-----------+------+-----+---------+-------+
6 rows in set (0.00 sec)

```

****
