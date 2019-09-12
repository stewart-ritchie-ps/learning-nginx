# Configuration

Configuration of NGINX is performed by editing the _.conf_ files in the NGINX root directory and sub-directories.

```
cd /etc/nginx/
ls -l
```

```
drwxr-xr-x 2 root root 4096 Aug 15 21:22 conf.d
-rw-r--r-- 1 root root 1007 Aug 13 08:50 fastcgi_params
-rw-r--r-- 1 root root 2837 Aug 13 08:50 koi-utf
-rw-r--r-- 1 root root 2223 Aug 13 08:50 koi-win
-rw-r--r-- 1 root root 5231 Aug 13 08:50 mime.types
lrwxrwxrwx 1 root root   22 Aug 13 08:50 modules -> /usr/lib/nginx/modules
-rw-r--r-- 1 root root  643 Aug 13 08:50 nginx.conf
-rw-r--r-- 1 root root  636 Aug 13 08:50 scgi_params
-rw-r--r-- 1 root root  664 Aug 13 08:50 uwsgi_params
-rw-r--r-- 1 root root 3610 Aug 13 08:50 win-utf
```

Let's take a look at the root _nginx.conf_ configuration file. The file is structured using _directives_ some of which are scoped into _blocks_. The root configuration file tells us about the locations of other interesting files, e.g. /var/log/nginx/error.log. The _include_ directive is used to inline other configuration files so we have reasonable way of structuring complex configurations and re-using configuration snippets.

```Nginx
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

In actual fact, we would seldom need to edit the root configuration file. The files we want are a level down in the _conf.d_ directory.

```
cd /etc/nginx/conf.d/
ls -l
```

```
-rw-r--r-- 1 root root 1093 Aug 13 08:50 default.conf
```

To start with there's a single file (_default.conf_), but we can create as many as we need - we might decide to have one file per server for example. For the purposes of learning, just work with the default configuration file.

```Nginx
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

## Exercises

* [Enable request logging](/md/enable-request-logging.md)
* [Add error logging](/md/add-error-logging.md)
* [Change root web page](/md/change-root-web-page.md)
