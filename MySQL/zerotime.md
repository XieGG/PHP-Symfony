### '0000-00-00 00:00:00' 时间格式报错
摘自:
> https://blog.csdn.net/zhengwei125/article/details/79003563
#
> show variables like 'sql_mode';

参数中含有:  no_zero_date

解决办法： 去掉 no_zero_date 

> set global sql_mode='strict_trans_tables,no_zero_in_date,error_for_division_by_zero,no_auto_create_user,no_engine_substitution';
