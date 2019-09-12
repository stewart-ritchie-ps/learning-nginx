# Directory alias

Sometimes we might want to break the relationship between request location and the file-system path - we can use a directory alias for this purpose.

Let's create a special location inside _/usr/web1/_ called _/deep/underground/_.

```
cd /usr/web1
mkdir -p /usr/web1/deep/underground
cp index.html /usr/web1/deep/underground/
```

The normal way to access this would be via it's implicit url.

```
curl -i http://localhost:8080/deep/underground/
```

But we can create an alias for this path.

```Nginx
location /special/ {
    alias /usr/web1/deep/underground/;
}
```

```
curl -i http://localhost:8080/special/
```

Now let's hide the direct path by returning a 404 for those requests.

```Nginx
location /deep/ {
    return 404;
}

location /special/ {
    alias /usr/web1/deep/underground/;
}
```
