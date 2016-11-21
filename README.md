# runverter-server

Welcome to the server backend of [Runverter](http://runverter.io).

The whole application runs in separated docker containers, which will be maintained in this repository and published as images on [Docker Hub](https://hub.docker.com/r/stefankracht/).

## architecture

All incoming web requests are loadbalanced by haproxy and spreaded to the underlying node containers.

The node applications connect to a redis instance to get the current index page. 

Assets of the applications are hosted via amazon S3 CDN.

## setting up the server

- SSH into a server.
- Make sure docker is installed. 

### create a shared network

```shell
$ docker network create runverter
```

### start redis

- Pull Redis image from docker and start container.
```shell
$ docker pull redis
$ docker run -d --name runverter-redis --net runverter -p 6379:6379 redis redis-server --appendonly yes
```
- This container will expose the port ```6379``` and its ip will become available in the shared network via the hostname ```runverter-redis```. This is required to let the node applications access Redis.

### start node application(s)

- Pull runverter-webserver and start 6 container instances.
```shell
$ docker pull stefankracht/runverter-webserver
$ docker run -d --name runverter-app-1 --net runverter stefankracht/runverter-webserver && docker run -d --name runverter-app-2 --net runverter stefankracht/runverter-webserver && docker run -d --name runverter-app-3 --net runverter stefankracht/runverter-webserver && docker run -d --name runverter-app-4 --net runverter stefankracht/runverter-webserver && docker run -d --name runverter-app-5 --net runverter stefankracht/runverter-webserver && docker run -d --name runverter-app-6 --net runverter stefankracht/runverter-webserver
```

### start haproxy

- Pull runverter-haproxy and start container.
```shell
$ docker pull stefankracht/runverter-haproxy
$ docker run -d --name runverter-haproxy --net runverter -p 80:80 stefankracht/runverter-haproxy
```
