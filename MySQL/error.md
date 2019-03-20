### MySQL & Ubuntu 的一些错误
##
### 导入数据 '0000-00-00 00:00:00' 时间格式报错
参数中含有:  NO_ZERO_DATE
解决办法： 去掉 NO_ZERO_DATE 

    mysql> select @@sql_mode; //查看sql_mode内容
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | @@sql_mode                                                                                                                                |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    1 row in set (0.00 sec)
    mysql> set global sql_mode='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'; //去掉NO_ZERO_DATE
    Query OK, 0 rows affected (0.00 sec)
    
### 使用 groupBy 错误 
Syntax error or access violation: 1055 Expression #1 of SELECT list is not in GROUP     
原因：     
MySQL 5.7.5和up实现了对功能依赖的检测。如果启用了only_full_group_by SQL模式(在默认情况下是这样)，那么MySQL就会拒绝选择列表、条件或顺序列表引用的查询，这些查询将引用组中未命名的非聚合列，而不是在功能上依赖于它们。(在5.7.5之前，MySQL没有检测到功能依赖项，only_full_group_by在默认情况下是不启用的。关于前5.7.5行为的描述，请参阅MySQL 5.6参考手册。)

    mysql> select @@sql_mode; //查看sql_mode内容
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | @@sql_mode                                                                                                                                |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    | ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
    +-------------------------------------------------------------------------------------------------------------------------------------------+
    1 row in set (0.00 sec)
    
    mysql> set global sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'; //去掉ONLY_FULL_GROUP_BY
    Query OK, 0 rows affected (0.00 sec)
### 上面的方法 只是临时修改,永久修改方法:
> 需要重启mysql服务:修改mysql的配置文件my.cnf,添加以下参数 
> sql_mode=ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION //实际是去除NO_ZERO_IN_DATE,NO_ZERO_DATE(根据需求去掉参数)