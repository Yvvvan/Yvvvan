---
layout: post
title: "在服务器部署Wordpress"
subtitle: ""
date: 2024-10-01
author: "Yvan"
header-img: "img/bg/wp-bg.jpg"
header-mask: 0.3
header-img-credit: "sheavi from Wallhaven"
header-img-credit-href:  "https://whvn.cc/426qqy"
no-catalog: false
mathjax: true
category: "code"
tags:

- Wordpress
- 后端
---
如果不用docker 直接在服务器部署Wordpress，可以参考以下步骤。

### 步骤 1: 准备服务器

1. **登录到root账户**：
   使用`su`命令切换到root账户，或者直接以root身份登录。

2. **创建新用户**：
   使用`useradd`命令来创建新用户，假设你想创建的用户是`newuser`，可以运行以下命令：
   
   ```bash
   useradd newuser
   ```

3. **设置密码**：
   为新用户设置一个密码：
   
   ```bash
   passwd newuser
   ```

4. **将用户添加到管理员组**：
   这样新用户就可以拥有管理权限。使用以下命令将用户添加到管理员组：
   
   ```bash
   usermod -aG sudo newuser    # Ubuntu
   usermod -aG wheel newuser   # CentOS
   ```

5. **验证**：
   你可以切换到新用户并验证权限：
   
   ```bash
   su - newuser 
   sudo whoami
   ```

6. **登录服务器**  
   使用 SSH 登录你的服务器。
   
   ```bash
   ssh newuser@server_ip
   ```

7. **更新系统软件包**
   ```bash
   sudo apt update && sudo apt upgrade -y # 针对基于Debian/Ubuntu的系统 
   sudo yum update -y # 针对基于CentOS/RHEL的系统
   ```

### 步骤 2: 安装 LAMP 堆栈

1. **安装 Apache** (如果使用nginx+php则不需要)
   
   ```bash
   sudo apt install apache2 # 针对Ubuntu/Debian 
   sudo yum install httpd # 针对CentOS/RHEL
   ```

2. **安装 MySQL/MariaDB**
   
   ```bash
   sudo apt install mysql-server # Ubuntu/Debian
   sudo yum install mariadb-server # CentOS/RHEL`
   ```
   
   有必要的话需要启动MariaDB, 设置权限
   
   ```bash
   sudo systemctl start mariadb # CentOS
   sudo systemctl status mariadb
   
   sudo chown mysql:mysql /var/lib/mysql/mysql.sock
   sudo chmod 755 /var/lib/mysql
   ```
   
   然后，启动 MySQL 并设置 root 密码 
   
   在第1处输入密码时，由于是新安装，请直接按 `enter`：
   
   ```bash
   sudo mysql_secure_installation
   ```

3. **安装 PHP**
   
   ```bash
   sudo apt install php libapache2-mod-php php-mysql # Ubuntu/Debian 
   sudo yum install php php-mysqlnd # CentOS/RHEL
   ```
   
   检查 php 版本
   
   ```bash
   php -v
   ```
   
   安装 php-fpm
   
   ```bash
   # Ubuntu/Debian
   sudo apt update
   sudo apt install nginx php-fpm php-mysql

   # CentOS/RHEL
   sudo yum install epel-release
   sudo yum install nginx php-fpm php-mysql
   ```

4. **重启 Apache**
   
   如有需要的话 可以更换端口：
   
   更改1 Apache监听端口：
   
   ```bash
   sudo nano /etc/apache2/ports.conf # Ubuntu/Debian
   sudo nano /etc/httpd/conf/httpd.conf # CentOS/RHEL
   ```
   
   ```apacheconf
   Listen 15678
   ```
   
   更改2 Apache虚拟主机配置：
   
   ```bash
   sudo nano /etc/apache2/sites-available/000-default.conf  #Ubuntu
   sudo nano /etc/httpd/conf.d/wordpress.conf # CentOS/RHEL 新建
   ```
   
   ```apacheconf
   <VirtualHost *:15678>
   
           ServerAdmin webmaster@localhost
           DocumentRoot /var/www/html/wordpress
   
           <Directory /var/www/html/wordpress>
                   Options Indexes FollowSymLinks
                   AllowOverride All       
                   Require all granted
           </Directory>
   
           # Ubuntu
           ErrorLog ${APACHE_LOG_DIR}/error.log
           CustomLog ${APACHE_LOG_DIR}/access.log combined
   
           # CentOS
           # ErrorLog /var/log/httpd/error.log
           # CustomLog /var/log/httpd/access.log combined

   </VirtualHost>
   
   ```
   
   更改3 Nginx配置：

   记得先备份

   ```bash
   sudo cp /etc/nginx/nginx.conf ./nginx.backup.conf
   ```

   ```bash
   server {
        listen 80;
        server_name domain.com;

        location / {
            gzip on;
            gzip_proxied any;
            proxy_pass http://127.0.0.1:15678/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
      }
   ```

   重启 Apache:

   ```bash
   sudo systemctl restart apache2 # Ubuntu/Debian
   sudo systemctl restart httpd # CentOS/RHEL
   ```

   重启 Nginx:

   ```bash
   sudo systemctl restart nginx
   ```


### 

### 步骤 3: 配置 MySQL 数据库

1. **登录 MySQL**
   
   ```bash
   sudo mysql -u root -p
   ```

2. **创建 WordPress 数据库和用户**
   
   ```sql
   CREATE DATABASE wordpress; 
   CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'password'; 
   GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost'; 
   FLUSH PRIVILEGES; 
   EXIT;
   ```

### 步骤 4: 下载并安装 WordPress

1. **下载 WordPress**
   
   ```bash
   cd /tmp 
   curl -O https://wordpress.org/latest.tar.gz
   ```

2. **解压 WordPress**
   
   ```bash
   tar xzvf latest.tar.gz
   ```

3. **将 WordPress 文件移动到服务器目录**
   
   ```bash
   sudo mv /tmp/wordpress /var/www/html/
   ```

4. **设置正确的权限**
   
   ```bash
   # Ubuntu
   sudo chown -R www-data:www-data /var/www/html/wordpress 
   sudo chmod -R 755 /var/www/html/wordpress
   
   # CentOS
   sudo chown -R apache:apache /var/www/html/wordpress
   ```

### 步骤 5: 配置 WordPress

1. **创建 WordPress 配置文件**
   
   ```bash
   cd /var/www/html/wordpress 
   cp wp-config-sample.php wp-config.php
   ```

2. **编辑配置文件，连接到数据库**
   
   ```bash
   sudo nano wp-config.php
   ```
   
   在文件中找到以下行，并根据你的数据库信息修改：
   
   ```php
   define('DB_NAME', 'wordpress'); 
   define('DB_USER', 'wpuser'); 
   define('DB_PASSWORD', 'password'); 
   define('DB_HOST', 'localhost');
   ```
   
   保存 `Ctrl+O` 退出 `Ctrl+X`

### 步骤 6: 完成安装

1. 在浏览器中访问你的服务器IP地址或域名或者绑定端口以后从`localhost`访问：

2. 按照屏幕上的提示完成安装，如设置站点名称、创建管理员账号等。

安装完成后，你可以通过 `/wp-admin` 进入 WordPress 后台。

### 步骤 7: Apache 转 php-fpm
按照以上的步骤，可以在localhost:15678访问wordpress, 也可以通过nginx转发到80端口，这样就可以通过域名访问了。

如果不能使用apache，或域名端口有问题，可以使用nginx+php-fpm的方式，这样就不需要apache了。

1. 配置Nginx

   ```nginx
   user www-data; ## CentOS 为 nginx
   http 
   server {
      server_name domain.com;

      root /var/www/html/wordpress;  # 确保这个路径正确指向你的 WordPress 文件夹
      index index.php index.html index.htm;

      location / {
         try_files $uri $uri/ /index.php?$args;
      }

      location ~ \.php$ {
         include snippets/fastcgi-php.conf;   # 如果没有配置文件可删除
         fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;   # 确保这个路径是 PHP 8.1 FPM 的套接字路径
         fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
         include fastcgi_params;
      }

      # 以上部分 也可以使用 tcp 方式
      # location ~ \.php$ {
      #   include fastcgi_params;
      #   fastcgi_pass 127.0.0.1:9000;  # PHP-FPM 监听的地址
      #   fastcgi_index index.php;
      #   fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
      #}

      location ~ /\.ht {
         deny all;
      }

      include       mime.types;
      default_type  application/octet-stream;
      types {
         application/javascript js;
      }

      # SSL 配置 (如果需要) Certbot自动配置
      listen 443 ssl; # managed by Certbot
      ssl_certificate /etc/letsencrypt/live/xxxx/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/xxxx/privkey.pem;
      include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
      ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
   }

   # HTTP 转 HTTPS, 如果没有SSL 可以删除
   # 并把 ‘linsten 80;’ 移到上面
   server {
      if ($host = xxxx) {
         return 301 https://$host$request_uri;
      } # managed by Certbot

      listen 80;
      server_name xxxx;
      return 404; # managed by Certbot

   }

   ```


2. 修改 php （Ubuntu, 如果需要）

   ```bash
   sudo nano /etc/php/8.1/fpm/pool.d/www.conf
   ```

   ```bash
   ls -l /var/run/php/ # 查看套接字路径
   ```

   ```bash
   listen = /var/run/php/php8.1-fpm.sock  # 套接字路径
   listen.owner = www-data  
   listen.group = www-data
   listen.mode = 0660
   ```

3. 修改权限(CentOS)

   权限从apache 改为 nginx

   ```bash
      sudo chown -R nginx:nginx /var/www/html/wordpress
      sudo find /var/www/html/wordpress -type d -exec chmod 755 {} \;  # 设置目录权限
      sudo find /var/www/html/wordpress -type f -exec chmod 644 {} \;  # 设置文件权限

      ##改回来
      sudo chown -R apache:apache /var/www/html/wordpress
      sudo systemctl restart httpd
   ```

   在Ubuntu中，都是www-data，所以不需要修改


4. 打开 php-fpm

   ```bash

   sudo systemctl stop apache2
   sudo systemctl disable apache2

   sudo systemctl start php8.1-fpm
   sudo systemctl enable php8.1-fpm

   sudo systemctl reload nginx

   # 改回来的话 把启动和停止的命令倒一下
   ```

5. Wordpress 配置

   把Wordpress中的URL改为域名, 也可以在后台设置

   ```bash
   nano /var/www/html/your_wordpress_directory/wp-config.php
   ```

   ```php
   define('WP_HOME', 'https://your_domain.com');
   define('WP_SITEURL', 'https://your_domain.com');
   ```

