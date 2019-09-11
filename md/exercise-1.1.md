# Enable web request logging (Ex 1.1)

Uncomment the _access_log_ directive in _default.conf_ and save.

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
