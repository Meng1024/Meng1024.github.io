---
layout: wiki
title: MySQL
categories: MySQL
description: MySQL 使用与技巧
keywords: Database
---

### Basic Commands
最开始是手动下载的安装包，然后出了问题，又找不到my.cnf，简直了。。 果断卸载。用brew了。。

install
```
brew update
brew install mysql
```

start, restart
```
mysql.server start
mysql.server restart
```

version
```
mysql -V
```

如果出现这个问题 mysql: command not found
```
ps aux | grep mysql
meng             43266   0.3  1.2  4942172 397432 s003  S    12:17AM   0:02.11 /usr/local/Cellar/mysql/8.0.16/bin/mysqld --basedir=/usr/local/Cellar/mysql/8.0.16 --datadir=/usr/local/var/mysql --plugin-dir=/usr/local/Cellar/mysql/8.0.16/lib/plugin --log-error=Mengs-MBP.T-mobile.com.err --pid-file=/usr/local/var/mysql/Mengs-MBP.T-mobile.com.pid

export PATH=/usr/local/Cellar/mysql/8.0.16/bin/mysqld:$PATH
```

login
```
 mysql -u root -p
```

### set timezone
登陆数据库后
```
set global time_zone=''+8:00';
```

```
show databases;
use $databasename;
show tables;
```

```
CREATE TABLE `miaosha`.`user_info` (
  `id` INT NOT NULL,
  `name` VARCHAR(64) NOT NULL,
  `gender` INT NOT NULL,
  `age` INT NOT NULL,
  `telphone` VARCHAR(45) NOT NULL,
  `register_mode` VARCHAR(45) NOT NULL,
  `third_party_id` VARCHAR(64) NOT NULL,
  `user_infocol` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`));
```
