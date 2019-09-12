# Location context

The _location_ context is one of the most frequently used - it allows us to match a request urls and perform various actions as a result, for example we could:

* Serve some content
* Rewrite a request
* Redirect a request
* Return error codes
* Proxy the request to another server

## Url anatomy

To understand how _location_ contexts match we should consider url anatomy. In a url like this

```
http://images.test.com:8080/hello/example1.jpg?size=large
```

We have the following parts:

* Protocal **http://**
* Subdomain **images**
* Host **test.com**
* Port **:8080**
* Path **/hello/example1.jpg**
* Query **?**
* Arguments **size=large**
* Fragment

The first thing to note is that the _location_ context is only concerned with the _path_ portion of the request url. The following example would match this request.

```Nginx
location /hello/ {
}
```
