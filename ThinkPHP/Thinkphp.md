## ThinkPHP 5.0 基础用法
### 数据库操作
    setInc(字段名,5) //自增一个字段的值,默认自增1.
    setDec(字段名,5) //自减一个字段的值,默认自减1.
    setInc/setDec //支持延时更新,如果需要延时更新则传入第三个参数.
    setInc(字段名,5,10) //延时10秒更新.
    value(字段名) //查询某个字段的值可以用.
    $userId = Db::name('user')->getLastInsID(); //数据添加完成 获取新增数据的主键(id)
