### Symfony 基础命令知识

	findOneBy(); //获取一条
	findAll(); //获取所有
	findBy(['createAt'=>'asc']); //排序
	$request->query->get('...'); //获取到数据

> $em = $this->getDoctrine()->getManager('rtu')->getRepository('RtuBundle:Device')->findOneBy(['address'=>$text['addr']]);

	getDoctrine();
	getManager('rtu'); //选择数据库(多数据库使用)
	getRepository('RtuBundle:Device'); //获数据表
	findOneBy(['表'=>$name]); //查询 表中的字段 和 变量字段相同的数据
	$em->getName() //获取服务参数

