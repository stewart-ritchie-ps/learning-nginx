# Locations and routing

Go ahead and amend your _default.conf_ so that it looks like this.

```Nginx
server {
    listen 80;
    server_name localhost;
    
    access_log /var/log/nginx/host.access.log main;
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
        root /usr/share/nginx/html;
    }    
}
```

Copy an index page to _/usr/web1/banana/banana.html_ - modify it so that you can identify that the right page is being served.

Then we can test:

* The file /usr/web1/index.html should be served for the location /.
* The file /usr/web1/banana/banana.html should be served for the location /banana/.

Let's override the _index_ directive for the root location, but don't add a file yet.

```Nginx
    location / {
        index root.html;
    }  
```

When we request the root page we get a **403 Forbidden** response from the server. Tailing the error log is informative. The default behaviour, when the index file cannot be found is to attempt to list the directory.

```
2019/09/12 09:48:27 [error] 134#134: *35 directory index of "/usr/web1/" is forbidden, client: 172.17.0.1, server: localhost, request: "GET / HTTP/1.1", host: "localhost:8080"
```

If we want to allow directory listing, we can add the _autoindex_ directive.

```Nginx
    location / {
        autoindex on;
        index root.html;
    }  
```

Check that you get a directory listing after ths change instead of a 403. 

Now let's add the expected file _/usr/web1/root.html_ and re-check - the page should be served and the _autoindex_ directive ignored for the root location.

## Locations can be nested

When location contexts are nested directives are inherited from parent contexts. Test this by moving the /banana/ location inside the root location and commenting its _index_ directive.

```Nginx
    location / {
        autoindex on;
        index root.html;

        location /banana/ {
            #index banana.html;
        }
    }
```
