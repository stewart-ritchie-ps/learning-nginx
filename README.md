# nginx-ref

Learning material for NGINX.

## Getting started

An easy approach to running NGINX is to pull and run the Docker image. We start by pulling the latest image and then run NGINX in detached server mode. The local 8080 port is mapped to the default port on which NGINX listens (80).

```
docker pull nginx
docker run -p 8080:80 -d nginx
```

You can connect to the container using the container id returned by _docker run_, or at any time by listing your docker containers.

```
docker container ls -a
```

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS                  NAMES           
42e6ba0fde39        nginx               "nginx -g 'daemon of…"   5 minutes ago       Up 5 minutes            0.0.0.0:8080->80/tcp   eloquent_hellman
```