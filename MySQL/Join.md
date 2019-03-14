### MySQL 连接查询语句
##
### Left join 实例
> SELECT a.name,a.username,b.name from user1 a LEFT JOIN user2 b on a.id=b.id where a.zt = '1'; //左连接查询