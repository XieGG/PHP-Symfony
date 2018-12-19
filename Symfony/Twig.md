### Twig 基础命令知识:
	{{ hello|raw }} //变量不被转义
	{{ app.request.query.get('address') }} //获取所有GET参数
	{{ ...|replace({'0':'正常','1':'停电'}) }} //代替
	{{ ...|date('Y-m-d H:i:s') }} //时间戳转为时间	
