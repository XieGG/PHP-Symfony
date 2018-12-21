### Symfony 基础命令知识

	findOneBy(); //获取一条
	findAll(); //获取所有
	findBy(['createAt'=>'asc']); //排序
	$request->query->get('...'); //获取到数据
	$this->getDoctrine()->getManager('rtu')->getRepository('RtuBundle:Device')->findOneBy(['address'=>$text['addr']]); //查库 查找数据
	getDoctrine();
	getManager('rtu'); //获取对象的控制器
	getRepository('RtuBundle:Device'); //获取类的数据库
	get('admin.department'); //获取service服务

