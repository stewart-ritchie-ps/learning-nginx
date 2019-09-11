# Change root web page

The default configuration defines that a static page is served for the root location.

```Nginx
  location / {
      root   /usr/share/nginx/html;
      index  index.html index.htm;
  }
```

So when we request _localhost:8080/_, we should look in the directory _/usr/share/nginx/html/_ for a file called _index.html_, or _index.htm_.

If you want, take a look at that directory and confirm the contents:

```
-rw-r--r-- 1 root root 494 Aug 13 08:50 50x.html
-rw-r--r-- 1 root root 612 Aug 13 08:50 index.html
```

## Create a new website

Create an empty directory for the new website.

```
mkdir /usr/web1
cd /usr/web1
```

Create a file called _index.html_ with some contents, e.g.

```Html
<!DOCTYPE html>
<html>
  <head>
    <title>Web 1</title>
  </head>
  <body>
    <p>Hello World!</p>
  </body>
</html>
```

Now edit the default configuration so that it serves the new page.

```Nginx
  location / {
      root   /usr/web1;
      index  index.html index.htm;
  }
```
