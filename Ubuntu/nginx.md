### Ubuntu 配置php+nginx+mysql环境
##
#### 安装PHP,MySQL
>$ sudo apt-get update //更新源    
>$ sudo apt-get install php //安装PHP(可以安装需要的扩展)      
>$ php -v //查看安装的PHP版本  
>$ sudo apt-get install mysql-server //安装MySQL  
>$ sudo apt-get install mysql-client    
>$ mysql -u root -p //进入MySQL   
#### 安装Nginx
>$ sudo apt-get update //更新源    
>$ sudo apt-get install nginx //安装Nginx   
>$ nginx -v //查看安装的Nginx版本(或者在浏览器输入 127.0.0.1)
#### 配置Nginx的多Web服务器 (Symfony)
>$ cd /etc/nginx/sites-available //进入nginx站点目录      

此处为 Symfony3.4 站点的配置

    server {
        server_name law.com www.law.com;    #域名
        root /var/www/law/web;  #站点代码目录
    
        location / {
            # try to serve file directly, fallback to app.php
            try_files $uri /app.php$is_args$args;
        }
        # DEV模式(上线请确保用户无法访问,或者直接删除DEV配置)
        # DEV
        # This rule should only be placed on your development environment
        # In production, don't include this and don't deploy app_dev.php or config.php
        location ~ ^/(app_dev|config)\.php(/|$) {
            fastcgi_pass 127.0.0.1:9000;    #可以配置 fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;(php-fpm的目录)
            fastcgi_split_path_info ^(.+\.php)(/.*)$;
            include fastcgi_params;
            # When you are using symlinks to link the document root to the
            # current version of your application, you should pass the real
            # application path instead of the path to the symlink to PHP
            # FPM.
            # Otherwise, PHP's OPcache may not properly detect changes to
            # your PHP files (see https://github.com/zendtech/ZendOptimizerPlus/issues/126
            # for more information).
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            fastcgi_param DOCUMENT_ROOT $realpath_root;
        }
        # PROD
        location ~ ^/app\.php(/|$) {
            fastcgi_pass 127.0.0.1:9000;    #可以配置 fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;(php-fpm的目录)
            fastcgi_split_path_info ^(.+\.php)(/.*)$;
            include fastcgi_params;
            # When you are using symlinks to link the document root to the
            # current version of your application, you should pass the real
            # application path instead of the path to the symlink to PHP
            # FPM.
            # Otherwise, PHP's OPcache may not properly detect changes to
            # your PHP files (see https://github.com/zendtech/ZendOptimizerPlus/issues/126
            # for more information).
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            fastcgi_param DOCUMENT_ROOT $realpath_root;
            # Prevents URIs that include the front controller. This will 404:
            # http://domain.tld/app.php/some-path
            # Remove the internal directive to allow URIs like this
            internal;
        }
    
        # return 404 for all other php files not matching the front controller
        # this prevents access to other php files you don't want to be accessible.
        location ~ \.php$ {
            return 404;
        }
    
        error_log /var/log/nginx/law_error.log; #错误日志路径
        access_log /var/log/nginx/law_access.log;   #日志路径
    }
> 配置完成保存 详情参照[Symfony官网](https://symfony.com/doc/3.4/setup/web_server_configuration.html)   
>$ cd /etc/nginx/sites-enabled //启用的站点目录(方便快速启用,停用)     
>$ sudo ln -s /etc/nginx/sites-available/law law //和 sites-available 的站点配置文件进行软连接   
>$ cd /etc/hosts //配置hosts      
Linux 下的权限分配    
>$ HTTPDUSER=$(ps axo user,comm | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1)         
>$ sudo setfacl -dR -m u:"$HTTPDUSER":rwX -m u:$(whoami):rwX var    
>$ sudo setfacl -R -m u:"$HTTPDUSER":rwX -m u:$(whoami):rwX var     
详情参照[Symfony官网](https://symfony.com/doc/3.4/setup/file_permissions.html)
### 其他的一些配置(网站的)
>$ 配置目录 /etc/php/7.2/fpm/pool.d/www.conf //可以配置 监听端口,用户,用户组     
>$ 配置php.ini /etc/php/7.2/fpm/php.ini //配置文件(根据需求配置)
