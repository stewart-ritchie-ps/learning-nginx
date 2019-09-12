# Exercise - Add error logging

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
curl -i http://localhost:8080/banana/
```

```
2019/09/06 15:57:19 [error] 474#474: *6 open() "/usr/share/nginx/html/banana" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /banana HTTP/1.1", host: "l$
```
