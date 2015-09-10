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
$ docker run -d --name runverter-redis -p 6379:6379 redis redis-server --appendonly yes
```
- This container will expose the port ```6379``` and adds its ip to the hostname ```runverter-redis```. This is required to let the node applications talk to Redis.

### start node application(s)

- Start 6 instances.
```shell
$ docker run --name runverter-app-1 -d stefankracht/runverter-webserver && docker run --name runverter-app-2 -d stefankracht/runverter-webserver && docker run --name runverter-app-3 -d stefankracht/runverter-webserver &&  docker run --name runverter-app-4 -d stefankracht/runverter-webserver && docker run --name runverter-app-5 -d stefankracht/runverter-webserver && docker run --name runverter-app-6 -d stefankracht/runverter-webserver
```

### start haproxy

- Start haproxy.
```shell
$ docker run --name runverter-haproxy -d -p 80:80 stefankracht/runverter-haproxy
```
