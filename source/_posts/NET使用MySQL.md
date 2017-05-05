---
title: .NET使用MySQL
date: 2017-05-04 23:42:28
tags: .NET
---
.NET 课程设计要用到数据库，可是.NET 默认使用的是 SQL Server，这个重量级的数据库软件，我电脑配置不高，运行起来很卡，所以我也就打心底里不想使用它。之前有一段时间有用过，不过已经永远丢失在一段硬盘改革的活动中了。所以我决定使用最亲民的 MySQL 。很多同学觉得 .NET 连接 MySQL 会很麻烦，宁愿下载安装 SQL Server，我只想并不比使用 SQL Server麻烦。下面我简单记录一下我的使用过程：

首先，要下载 **MySql.Data.dll** 文件（这个网上搜会有）

然后，在项目目录中新建一个 **bin** 文件夹，并把 **MySql.Data.dll** 放进去。

在需要连接数据库的 .aspx.cs 文件中使用 **using MySql.Data.MySqlClient;**引入 连接 MySQL 的相关函数

最后用：
```
string conn = "Data Source=127.0.0.1;User ID=root;Password=root;DataBase=db123";
MySqlConnection myconn = new MySqlConnection(conn);
myconn.Open();
string sql = "select * from table“;
MySqlCommand cmm = new MySqlCommand(sql, myconn);
myconn.Close();
```

再改改相关配置就可以访问数据库和查询数据库中的信息了！！