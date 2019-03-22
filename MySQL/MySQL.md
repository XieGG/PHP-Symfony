### MySQL 基础命令知识

	DELETE from device_state; //删除数据
	truncate table `表名`; //重新排序从1开始：会删除数据
	UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值 //更新某列数据
	UPDATE 表名 set 列名 = replace(列名,'值','更新后的值') where 列名 like '%值%'; //更新数据(包含 值)
	DELETE FROM 表名称 WHERE 列名称 = 值 //删除数据
	
### SQL 修改 数据中 某个字段 中 包含某个值
> update 表名 SET 字段名 = REPLACE( 字段名, '需要更新的值', '更新之后的值' ) where 字段名 like '%需要更新的值%';