# Change root web page

The default configuration defines that a static page is served for the root location.

```Nginx
  location / {
      root   /usr/share/nginx/html;
      index  index.html index.htm;
  }
```

So when we request _localhost:8080/_, we should look in the directory _/usr/share/nginx/html_ for a file called _index.html_, or _index.htm_.
