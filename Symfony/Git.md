### Git 基础命令知识

	composer install //更新项目
	php bin/console doctrine:generate:entity //生成Entity
	php bin/console cache:clear //清除缓存
	php bin/console cache:clear --env=prod //清除生产环境的缓存
	php bin/console debug:container //查看服务
	php bin/console doctrine:database:create //创建数据库
	php bin/console doctrine:database:create --connection=库名 //创建数据库
	php bin/console doctrine:schema:update --force //更新数据表
	php bin/console doctrine:schema:update --force --em=customer //更新数据表
	git config --global credential.helper store //免除密码

### Entity 表命令

生成Entity表
>$ php bin/console doctrine:generate:entity  

更新Entity表
>$ php bin/console doctrine:schema:update --force (--em=库名) 
	
生成get set方法
>$ php bin/console doctrine:generate:entities GovernmentBundle:GzBgzyjgbshzfb

### Git 本地文件提交 Github
在Github上创建项目

本地文件夹 创建 .git 文件
>$ git init

将所有文件添加到本地仓库
>$ git add .

提交文件注释
>$ git commit -m "注释"

关联Github仓库 (只需要关联一次)
>$ git remote add origin https://github.com/XieGG/Study.git

上传本地代码
>$ git push origin master

下拉仓库代码
>$ git pull origin master




