# nginx-ref

Learning material for NGINX.

## Running in Docker

An easy approach to running NGINX is to pull and run the Docker image. We start by pulling the latest image and then run NGINX in detached server mode. The local 8080 port is mapped to the default port on which NGINX listens (80). Check that it is running by browsing http://localhost:8080/ and seeing the NGINX welcome page.

```
docker pull nginx
docker run -p 8080:80 -d nginx
```

You can connect to the container using the container id returned by _docker run_, or at any time by listing your docker containers.

```
docker container ls -a
```

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS                  NAMES
42e6ba0fde39        nginx               "nginx -g 'daemon of…"   5 minutes ago       Up 5 minutes            0.0.0.0:8080->80/tcp   eloquent_hellman
```

```
docker exec -it 42e6ba0fde39 bash
```

You should now be connected to the NGINX instance with bash. By default you get root access with Docker (no _sudo_ needed).

```
root@42e6ba0fde39:/#
```

## Installing a file editor

Docker images are very bare, so we need to install a file editor. Start by updating the package list. Then, install an editor. _Nano_ is a good choice for beginners but _vim_ tends to be used more commonly. 

```
apt-get update
apt-get install nano
apt-get install vim
```

Note: if you need to familiarise yourself with the editor, do so now (we won't cover those details here).

## NGINX configuration files

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

## NGINX contexts

Configuration directives are grouped into contexts that scope their application allowing us to serve a specific file for a specific web request, or perform a redirect.

The default files have the following structure.

```Nginx
# implicit main context

# events context 
# - controls how NGINX integrates with host OS.
# - only one instance of this context allowed.
events {
}

# http context
# - handles http(s) traffic
# - common settings for all servers.
http {

    # server context
    # - settings for a specific virtual web server.
    server {
  
        # location context
        # - setting for a specific request location.
        location / {
        }
    }
}
```

When contexts are nested like this, directives may be implicitly inherited from parent contexts, or overridden in child contexts.

## Exercises

[1. Enable web request logging](/exercise-1)

### Exercise 1. Enable web request logging

Let's take a look at the log files mentioned in the configuration.

```
cd /var/log/nginx/
ls -l
```

```
lrwxrwxrwx 1 root root 11 Aug 15 21:22 access.log -> /dev/stdout
lrwxrwxrwx 1 root root 11 Aug 15 21:22 error.log -> /dev/stderr
```

Now let's uncomment the _access_log_ directive in _default.conf_ and save.

```Nginx
server {
    listen       80;
    server_name  localhost;
    access_log  /var/log/nginx/host.access.log  main;
    
    # ...
}
```

Exit to basgh and signal NGINX to reload the cofiguration - it should report _signal process started_, if you get any errors check for syntax errors in your _.conf_ files.

```
nginx -s reload
```

Tail the log file, then issue some requests from your browser (CTRL+C to exit tail).

```
tail -f /var/log/nginx/host.access.log
```

```
172.17.0.1 - - [06/Sep/2019:15:29:48 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
172.17.0.1 - - [06/Sep/2019:15:40:32 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
172.17.0.1 - - [06/Sep/2019:15:40:40 +0000] "GET /banana HTTP/1.1" 404 555 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36" "-"
```

### Exercise 2. Add error log

Add the _error_log_ directive pointing to a new file.

```Nginx
server {
    listen       80;
    server_name  localhost;
    access_log  /var/log/nginx/host.access.log  main;
    error_log /var/log/nginx/host.error.log error;
    
    # ...
}
```

Reload...

```
nginx -s reload
```

Request an unmapped location to generate a 404, then examine the new error log file.

```
2019/09/06 15:57:19 [error] 474#474: *6 open() "/usr/share/nginx/html/banana" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /banana HTTP/1.1", host: "l$
```
