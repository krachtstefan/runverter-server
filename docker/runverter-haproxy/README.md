# runverter-haproxy

This container serves a haproxy to loadbalance all incoming web requests to the ```runverter-webserver``` container cluster.

## Requirements

The node cluster should be available on the following addresses.

```
runverter-app-1:3000
runverter-app-2:3000
runverter-app-3:3000
runverter-app-4:3000
runverter-app-5:3000
runverter-app-6:3000
```

## Run container

```shell
$ docker pull stefankracht/runverter-haproxy
$ docker run -d --name runverter-haproxy --net runverter -p 80:80 stefankracht/runverter-haproxy
```

This starts the haproxy server in the shared network ```runverter``` and exposes port 80 to the outside world.

## Build the image 

```shell
$ docker build --tag stefankracht/runverter-haproxy .
```

## Publish the image

```shell
$ docker push stefankracht/runverter-haproxy
```
