//获得最近的软件包的列表
sudo apt-get update 

//安装Nginx服务器
sudo apt-get install nginx 

//安装php5
sudo apt-get install php5-fpm 

//安装php5在命令行运行的接口
sudo apt-get install php5-cli 

//安装php5加密拓展库
sudo apt-get install php5-mcrypt 

//安装php的MySQL驱动
sudo apt-get install php5-mysql 

//安装MySQL
sudo apt-get install mysql-server

//安装git神器
sudo apt-get install git 

//修改php配置文件
sudo vim /etc/php5/fpm/php.ini
cgi.fix_pathinfo=0

//使用 php5enmod 启用 MCrypt 扩展
sudo php5enmod mcrypt

//重启 php5-fpm 服务
sudo service php5-fpm restart

//nginx配置
server {
    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;

    # 设定网站根目录
    root /var/www/laravel/public;
    # 网站默认首页
    index index.php index.html index.htm;

    # 服务器名称，server_domain_or_IP 请替换为自己设置的名称或者 IP 地址
    server_name server_domain_or_IP;

    # 修改为 Laravel 转发规则，否则PHP无法获取$_GET信息，提示404错误
    location / {
        try_files $uri $uri/ /index.php?$query_string;        
    }

    # PHP 支持
    location ~ \.php$ {
        try_files $uri /index.php =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}

//重启 nginx 服务
sudo service nginx restart


