---
title: MySql-修改密码
date: 2023-12-18 16:41:22
tags: [MySql]
categories: MySql
cover: https://gitee.com/zyjgyp/imgs/raw/master/imgs/MySql-修改密码.webp
---

1. 登录 MySql

```Bash
mysql -u root -p
```

输入当前`root`用户的密码登录。

2. 修改密码

```Bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';
```

3. 刷新权限

```Bash
FLUSH PRIVILEGES;
```

4. 退出 MySql

```Bash
exit;
```
