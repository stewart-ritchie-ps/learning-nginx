# Contexts

Configuration directives are grouped into contexts that scope their application allowing us to serve a specific file for a specific web request, or perform a redirect.

The default files have the following effective structure (albeit split across multiple files).

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

In the default configuration the _root_ and _index_ directives are declared within the _location / {}_ context, meaning that we would have to repeat these in each subsequent _location_ context.

```Nginx
    server {
        location / {
            root /usr/web1;
            index index.html index.htm;
        }
    }
```

But, we could also move these directives to the _server_ context, where they would be inherited by all child contexts.

```Nginx
    server {
        root /usr/web1;
        index index.html index.htm;

        location / {
        }
    }
```

Now we can override the _index_ directive for a different location.

```Nginx
    server {
        root /usr/web1;
        index index.html index.htm;

        location / {
        }

        location /banana/ {
            index banana.html;
        }
    }
```

Note: your file is expectd to be _/usr/web1/banana/banana.html_ in the above example.

## Exercises

* [Locations and routing](/md/exercise-locations-and-routing.md)
