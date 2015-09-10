# runverter-haproxy

This container serves a haproxy to loadbalance all incoming web requests to the ```runverter-webserver``` container cluster.

## requirements

The node cluster should be available on the following addresses.

```
runverter-app-1:3000
runverter-app-2:3000
runverter-app-3:3000
runverter-app-4:3000
runverter-app-5:3000
runverter-app-6:3000
```

## run container

```shell
$ docker run --name runverter-haproxy -d stefankracht/runverter-haproxy
```

## build the image 

```shell
$ docker build --tag stefankracht/runverter-haproxy .
```

## publish the image

```shell
$ docker push stefankracht/runverter-haproxy
```
