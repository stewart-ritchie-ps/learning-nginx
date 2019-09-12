# Locations and routing

Go ahead and amend your _default.conf_ so that it looks like this.

```Nginx
server {
    listen 80;
    server_name localhost;
    
    access_log  /var/log/nginx/host.access.log main;
    error_log /var/log/nginx/host.error.log error;
    
    root /usr/web1;
    index index.html index.htm;
    
    location / {
    }
    
    location /banana/ {
        index banana.html;
    }
    
    error_page 500 502 503 504 /50x.html;
    
    location = /50x.html {
        root   /usr/share/nginx/html;
    }    
}
```

Copy and modify an index page as _/usr/web1/banana/banana.html_ so that you can identify that the right page is being served.

Then we can test:

* The file /usr/web1/index.html should be served for the location /.
* The file //usr/web1/banana/banana.html should be served for the location /banana/.
