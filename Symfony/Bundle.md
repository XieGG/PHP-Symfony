## 关于Bundle创建
### 建立Bundle
	PHP bin/console generate:bundle --namespace=LoginBundle //建立 名为 LoginBundle 的 Bundle
### 修改composer.json配置文件(自Symfony3.3)
	 "autoload": {
	        "psr-4": {
	            "AppBundle\\": "src/AppBundle",
	        },
	        "classmap": [ "app/AppKernel.php", "app/AppCache.php" ]
	    },
### 加入Bundle配置
	 "autoload": {
	        "psr-4": {
	            "AppBundle\\": "src/AppBundle",
	            "LoginBundle\\": "src/LoginBundle"
	        },
	        "classmap": [ "app/AppKernel.php", "app/AppCache.php" ]
	    },
### 更新composer自动加载文件
	composer dump-autoload
### Bundle目录结构
	Command --存放控制台命令
	Controller --控制器
	Entity --实体类
	Form --表单
	Repository --数据库查询
	Resources --routing表 && 前端Twig文件
	Service --服务
	-------未完------