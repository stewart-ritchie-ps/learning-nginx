# Change root web page

The default configuration defines that a static page for any location that starts with '/'. In actual fact thats any url we request.

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

```
nano /etc/nginx/conf.d/default.conf
```

```Nginx
  location / {
      root   /usr/web1;
      index  index.html index.htm;
  }
```

Save and reload NGINX configuration.

```
nginx -s reload
```

Browse to [localhost:8080](http://localhost:8080/), and confirm the page displays.

## Adding another page

Our existing location rule is actually all that is needed to route the entire website (assuming that urls map to file paths relative to the root folder.

If we want to serve a page for _/banana/_, we should create a sub-directory in our root and add an _index.html_ file.

```
mkdir /usr/web1/banana
cp /usr/web1/index.html /usr/web1/banana
```

Edit the new _/banana/index.html_ file so that you can confirm the behaviour.

```Html
<html>
  <head>
    <title>Web 1</title>
  </head>
  <body>
    <p>Have a banana!</p>
  </body>
</html>
```

Browse to [localhost:8080/banana/](http://localhost:8080/banana/).
