### ngnix.conf

###### 定义Nginx运行的用户和用户组
user www www;

###### #nginx进程数，建议设置为等于CPU总核心数。
worker_processes 8;

###### 全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
error_log /var/log/nginx/error.log info;

###### 进程文件
pid /var/run/nginx.pid;

###### 一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（系统的值ulimit-n）与nginx进程数相除，但是nginx分配请求并不均匀，所以建议与ulimit-n的值保持一致。
worker_rlimit_nofile 65535;

###### 工作模式与连接数上限
events

	{
	###### 参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ];epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
	use epoll;
	###### 单个进程最大连接数（最大连接数=连接数*进程数）
	worker_connections 65535;
	}

###### 设定http服务器
http
{

	include mime.types; #文件扩展名与文件类型映射表
	default_type application/octet-stream; #默认文件类型
	#charset utf-8; #默认编码
	server_names_hash_bucket_size 128; #服务器名字的hash表大小
	client_header_buffer_size 32k; #上传文件大小限制
	large_client_header_buffers 4 64k; #设定请求缓
	client_max_body_size 8m; #设定请求缓
	sendfile on;
	
	#开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为
	on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
	autoindex on; #开启目录列表访问，合适下载服务器，默认关闭。
	tcp_nopush on; #防止网络阻塞
	tcp_nodelay on; #防止网络阻塞
	keepalive_timeout 120; #长连接超时时间，单位是秒
	
	#FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
	fastcgi_connect_timeout 300;
	fastcgi_send_timeout 300;
	fastcgi_read_timeout 300;
	fastcgi_buffer_size 64k;
	fastcgi_buffers 4 64k;
	fastcgi_busy_buffers_size 128k;
	fastcgi_temp_file_write_size 128k;
	
	#gzip模块设置
	gzip on; #开启gzip压缩输出
	gzip_min_length 1k; #最小压缩文件大小
	gzip_buffers 4 16k; #压缩缓冲区
	gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
	gzip_comp_level 2; #压缩等级
	gzip_types text/plain application/x-javascript text/css application/xml;
	#压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
	gzip_vary on;
	#limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用
	
	upstream blog.ha97.com {
	#upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
	server 192.168.80.121:80 weight=3;
	server 192.168.80.122:80 weight=2;
	server 192.168.80.123:80 weight=3;
	}

	#虚拟主机的配置
	server
	{
	#监听端口
	listen 80;
	#域名可以有多个，用空格隔开
	server_name www.ha97.com ha97.com;
	index index.html index.htm index.php;
	root /data/www/ha97;
	location ~ .*.(php|php5)?$
	{
	fastcgi_pass 127.0.0.1:9000;
	fastcgi_index index.php;
	include fastcgi.conf;
	}
	#图片缓存时间设置
	location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
	{
	expires 10d;
	}
	#JS和CSS缓存时间设置



### Nginx禁止IP直接访问、防止域名恶意解析。

### server_name

Nginx中的server_name指令主要用于配置基于名称的虚拟主机，server_name指令在接到请求后的匹配顺序分别为：
1、准确的server_name匹配，例如：
 
server {
     listen       80;
     server_name  domain.com  www.domain.com;
     ...
}
 
 
2、以*通配符开始的字符串：

server {
     listen       80;
     server_name  *.domain.com;
     ...
}
3、以*通配符结束的字符串：

server {
     listen       80;
     server_name  www.*;
     ...
}
4、匹配正则表达式：

server {
     listen       80;
     server_name  ~^(?.+)\.domain\.com$;
     ...
}
nginx将按照1,2,3,4的顺序对server name进行匹配，只有有一项匹配以后就会停止搜索，所以我们在使用这个指令的时候一定要分清楚它的匹配顺序（类似于location指令）。
server_name指令一项很实用的功能便是可以在使用正则表达式的捕获功能，这样可以尽量精简配置文件，毕竟太长的配置文件日常维护也很不方便。下面是2个具体的应用：
1、在一个server块中配置多个站点：
server
   {
     listen       80;
     server_name  ~^(www\.)?(.+)$;
     index index.php index.html;
     root  /data/wwwsite/$2;
   }
站点的主目录应该类似于这样的结构：

/data/wwwsite/domain.com
/data/wwwsite/nginx.org
/data/wwwsite/baidu.com
/data/wwwsite/google.com
 

这样就可以只使用一个server块来完成多个站点的配置。

2、在一个server块中为一个站点配置多个二级域名。

实际网站目录结构中我们通常会为站点的二级域名独立创建一个目录，同样我们可以使用正则的捕获来实现在一个server块中配置多个二级域名：

 
server
   {
     listen       80;
     server_name  ~^(.+)?\.domain\.com$;
     index index.html;
     if ($host = domain.com){
         rewrite ^ http://www.domain.com permanent;
     }
     root  /data/wwwsite/domain.com/$1/;
   }
站点的目录结构应该如下：

/data/wwwsite/domain.com/www/
/data/wwwsite/domain
.com/nginx/
这样访问www.domain.com时root目录为/data/wwwsite/domain.com/www/，nginx.domain.com时为/data/wwwsite/domain.com/nginx/，以此类推。

### Nginx 的临时维护页面
每当服务器遇到 502 代码时，就自动转到临时维护的静态页：

server {
     listen 80;
     server_name mydomain.com;

     # ... 省略掉 N 行代码


     error_page 502 = @tempdown;

     location @tempdown {
         rewrite ^(.*)$ /pages/maintain.html break;
     }
}
如果你只想要【临时维护页面】就这样写（适合服务器更新东西或者改版）：

server {
     listen 80;
     server_name mydomain.com;

     # ... 省略掉 N 行代码

     # 所有页面都转跳到维护页
     rewrite ^(.*)$ /pages/maintain.html break;

}

rewrite 正则表达式 替换目标 flag标记
flag标记可以用以下几种格式：
last - 基本上都用这个Flag。
break - 中止Rewirte，不在继续匹配
redirect - 返回临时重定向的HTTP状态302
permanent - 返回永久重定向的HTTP状态301

捕获
(target) 匹配target,并捕获文本到自动命名的组里
(?target) 匹配target,并捕获文本到名称为name的组里，也可以写成(?’name’target)
(?:target) 匹配target,不捕获匹配的文本，也不给此分组分配组号

零宽断言
(?=target) 匹配target前面的位置
(?<=target) 匹配target后面的位置
(?!target) 匹配后面跟的不是target的位置
