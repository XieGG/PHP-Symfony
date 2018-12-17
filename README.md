# PHP-Symfony
PHP/Symfony-学习笔记
#Symfony:
-	findOneBy(); //获取一条
-	findAll(); //获取所有
-	findBy(['createAt'=>'asc']); //排序
-	$request->query->get('...'); //获取到数据
>	$this->getDoctrine()->getManager('rtu')->getRepository('RtuBundle:Device');<br>
>	getDoctrine();<br>
>	getManager('rtu'); //获取对象的控制器<br>
>	getRepository('RtuBundle:Device'); //获取类的数据库<br>

	
###MySql:
-	DELETE from device_state; //删除数据
-	truncate table `表名`; //重新排序从1开始：会删除数据
	
###Twig 相关:
-	{{ hello|raw }} //变量不被转义
-	{{ app.request.query.get('address') }} //获取所有GET参数
-	{{ ...|replace({'0':'正常','1':'停电'}) }} //代替
-	{{ ...|date('Y-m-d H:i:s') }} //时间戳转为时间	

###PHP函数:
-	dump();exit; //调试
-	json_encode(); //对变量进行 JSON 编码
-	format('y-m-d H:i'); //时间格式
-	explode(); //字符串转为数组
-	file_put_contents() //字符串写入文件中
-	hex2bin //16进制转为2进制

###Git:
-	composer install //更新项目
-	php bin/console doctrine:generate:entity //生成Entity
-	php bin/console cache:clear //清除缓存
-	php bin/console cache:clear --env=prod //清除生产环境的缓存
-	php bin/console debug:container //查看服务
-	php bin/console doctrine:database:create //创建数据库
-	php bin/console doctrine:database:create --connection=库名 //创建数据库
-	php bin/console doctrine:schema:update --force //更新数据表
-	php bin/console doctrine:schema:update --force --em=customer //更新数据表
-	git config --global credential.helper store //免除密码
-	php bin/console doctrine:schema:update --force --em=库名 //更新Entity表

###HTML:
-	input type="text" style="width:500px;border:0px;" value="xxxxxxx" ---//隐藏文本框边框

###编码规范:
-	service id 全部小写
-	表名 小写
