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

### start redis

- Pull Redis image from docker and start container.
```shell
$ docker pull redis
$ docker run -d --name runverter-redis -h runverter-redis -p 6379:6379 redis redis-server --appendonly yes
```
- This container will expose the port ```6379``` and adds its ip to the hostname ```runverter-redis```. This is required to let the node applications talk to Redis.

### start node application(s)

- Pull runverter-webserver and start 6 container instances.
```shell
$ docker pull stefankracht/runverter-webserver
$ docker run --name runverter-app-1 -h runverter-app-1 -P --link runverter-redis -d stefankracht/runverter-webserver && docker run --name runverter-app-2 -h runverter-app-2 -P --link runverter-redis -d stefankracht/runverter-webserver && docker run --name runverter-app-3 -h runverter-app-3 -P --link runverter-redis -d stefankracht/runverter-webserver && docker run --name runverter-app-4 -h runverter-app-4 -P --link runverter-redis -d stefankracht/runverter-webserver && docker run --name runverter-app-5 -h runverter-app-5 -P --link runverter-redis -d stefankracht/runverter-webserver && docker run --name runverter-app-6 -h runverter-app-6 -P --link runverter-redis -d stefankracht/runverter-webserver
```

### start haproxy

- Pull runverter-haproxy and start container.
```shell
$ docker pull stefankracht/runverter-haproxy
$ docker run --name runverter-haproxy --link runverter-app-1 --link runverter-app-2 --link runverter-app-3 --link runverter-app-4 --link runverter-app-5 --link runverter-app-6 -d -p 80:80 stefankracht/runverter-haproxy
```
