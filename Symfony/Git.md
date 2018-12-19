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
	php bin/console doctrine:schema:update --force --em=库名 //更新Entity表