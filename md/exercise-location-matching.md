# Location matching

So far we have used location contexts that match locations that start with a certain pattern. For example this pattern, which would match both [localhost:8080/banana/](http://localhost:8080/banana/) and [localhost:8080/banana/icecream/](http://localhost:8080/banana/icecream/).

```Nginx
    location /banana/ {
        index banana.html;
    }
```

Create the _/usr/web1/banana/icecream/banana.html_ file so that you can test this (modify the new file so you can tell it's being served rather than another file). Both links above should now serve their respective pages.

## Exact matching

If we need to match a location exactly we can modify the location directive with the **=** operator like this.

```Nginx
    location = /banana/ {
        index banana.html;
    }
```

The _/banana/_ location will function as before. But _/banana/icecream/_ is no longer matched and we fall back to the default for an unmatched path (you might get a directory listing, or a 403, depending on other settings).

## Location matches do not apply to the url query

With exact matching in place, it's a good time to demonstrate that the query portion of the url is ignored when it comes to location matching.

Try browsing [localhost:8080/banana/?t=12](http://localhost:8080/banana/?t=12) - the /usr/web1/banana/banana.html page should still serve.

This makes sense, because query parameters can be specified in any order, so matching against them is not easy.

As we'll see this also applies for regular expression matching. Put simply, the _location_ directive only matches against the _location_ portion of the requested _url_.

## Regular expression matching

We can match using a regular expression by using the **~** operator like this.

```Nginx
    location ~ /[Bb]anana/ {
        index banana.html;
    }
```

In order for this to work, we'll need to create _/usr/web1/Banana/banana.html_. Then try...

```
curl http://localhost:8080/banana
curl http://localhost:8080/Banana
```
