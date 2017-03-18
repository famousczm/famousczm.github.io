---
title: Oracle复制另一用户下的表
date: 2016-10-26 14:31:29
tags: Oracle
---
Oracle下要将用户A里的表T复制给用户B，首先要登陆用户A，用如下语句把查询表T的权限赋给用户B：

```
GRANT SELECT ON T TO A;
```

然后登陆用户B，复制表T并命名为S：

```
CREATE TABLE S AS SELECT * FROM A.T;
```

这样就可以了（表名前一定要加上模式名）