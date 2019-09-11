## NGINX contexts

Configuration directives are grouped into contexts that scope their application allowing us to serve a specific file for a specific web request, or perform a redirect.

The default files have the following structure.

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
