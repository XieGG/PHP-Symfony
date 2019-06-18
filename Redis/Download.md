### Redis 安装
在 Ubuntu 系统安装 Redis 可以使用以下命令:
> $ sudo apt-get update  
> $ sudo apt-get install redis-server  
      
启动 Redis
> $ redis-server  
  
查看 redis 是否启动？
> $ redis-cli

[详细参照](https://www.runoob.com/redis/redis-install.html)

### PHPRedis 扩展 安装
1.下载phpredis扩展文件
> $ git clone https://github.com/phpredis/phpredis.git  
  
2.移动文件  
> $ mv phpredis /etc/ 

3.安装
> $ cd /etc/phpredis  
> $ phpize    

如果phpize命令没有响应，可能是没有安装php-dev。我目前安装的是php7.2，键入命令
> $ apt-get install php7.2-dev

4.编译
> $ ./configure   
> $ make & make install

5.修改配置文件(php.ini)
> $ sudo gedit /etc/php/7.2/fpm/php.ini  
 
添加一行    
> $ extension=/etc/phpredis/modules/redis.so
