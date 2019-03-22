### Ubuntu 配置nginx一般的站点
##
>$ cd /etc/nginx/sites-available //进入nginx的站点目录    
>$ sudo gedit php   
    server {
           listen 80;
           listen [::]:80;
           server_name php.com www.php.com;
    
           # 定义服务器的默认网站根目录位置
           root /var/www/php/;
           
           # Add index.php to the list if you are using PHP
           index index.html index.htm index.nginx-debian.html;
    
           # access log file 访问日志
           access_log /var/log/nginx/php_access.log;
           
           # 禁止访问隐藏文件
           # Deny all attempts to access hidden files such as .htaccess, .htpasswd, .DS_Store (Mac).
           location ~ /\. {
                    deny all;
                    access_log off;
                    log_not_found off;
           }
        
           # 默认请求
           location / {
                    # 首先尝试将请求作为文件提供，然后作为目录，然后回退到显示 404。
                    # try_files 指令将会按照给定它的参数列出顺序进行尝试，第一个被匹配的将会被使用。
                    # try_files $uri $uri/ =404;
          
                    try_files $uri $uri/ /index.php?path_info=$uri&$args =404;
                    access_log off;
                    expires max;
           }
        
           # 静态文件，nginx 自己处理
           location ~ ^/(images|javascript|js|css|flash|media|static)/ {
                
               #过期 30 天，静态文件不怎么更新，过期可以设大一点，
               #如果频繁更新，则可以设置得小一点。
               expires 30d;
           }
        
           # .php 请求
           location ~ \.php$ {
                    try_files $uri =404;
                    include /etc/nginx/fastcgi_params;
                    fastcgi_pass 127.0.0.1:9000;
                    fastcgi_index index.php;
                    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                    fastcgi_intercept_errors on;
           }
        
          # PHP 脚本请求全部转发到 FastCGI 处理. 使用 FastCGI 默认配置.
          # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
          #
          #location ~ \.php$ {
          #       include snippets/fastcgi-php.conf;
          #
          #       # With php7.0-cgi alone:
          #       fastcgi_pass 127.0.0.1:9000;
          #       # With php7.0-fpm:
          #       fastcgi_pass unix:/run/php/php7.0-fpm.sock;
          #}
          
          # 拒绝访问. htaccess 文件，如果 Apache 的文档根与 nginx 的一致
          # deny access to .htaccess files, if Apache's document root
          # concurs with nginx's one
          #
          #location ~ /\.ht {
          #       deny all;
          #}
    }
>$ cd /etc/nginx/sites-enabled //进入nginx启用站点目录  
>$ sudo ln -s /etc/nginx/sites-available/php php //启用php站点    
>$ sudo gedit /etc/hosts //添加 127.0.0.1 php.com

权限和其他的配置可以查看上一篇文章。end