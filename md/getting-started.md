# Getting started

The easiest approach to learning NGINX is to run a local Docker container.

## Running in Docker

Start by pulling the latest image and then run it in detached server mode.

```
docker pull nginx
docker run -p 8080:80 -d nginx
```

The local 8080 port is mapped to the default port on which NGINX listens (80). Check that NGINX is running by browsing to [localhost:8080/](http://localhost:8080/) and seeing the NGINX welcome page.

You can connect to the container using the container id returned by _docker run_, or at any time by listing your docker containers.

```
docker container ls -a
```

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                  PORTS                  NAMES
42e6ba0fde39        nginx               "nginx -g 'daemon of…"   5 minutes ago       Up 5 minutes            0.0.0.0:8080->80/tcp   eloquent_hellman
```

```
docker exec -it 42e6ba0fde39 bash
```

You should now be connected to the NGINX instance with bash. By default you get root access with Docker (no _sudo_ needed).

```
root@42e6ba0fde39:/#
```

Once your container is running you can come back to it during subsequent sessions and pick up where you left off. If the container stops, just restart it.

```
docker container start 42e6ba0fde39
```

## Installing a file editor

Docker images are very bare, so we need to install a file editor. Start by updating the package list. Then, install an editor. _Nano_ is a good choice for beginners but _Vim_ tends to be used more commonly.

```
apt-get update
apt-get install nano
apt-get install vim
```

Note: familiarise yourself with your chosen editor if needed (we won't cover it here).
