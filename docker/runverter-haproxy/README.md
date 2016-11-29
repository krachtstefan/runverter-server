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
$ docker run -d --name runverter-haproxy --net runverter -v /etc/haproxy/certs/:/etc/haproxy/certs/ -p 80:80 -p 443:443 -p127.0.0.1:8080:8080 stefankracht/runverter-haproxy
```

This starts the haproxy server in the shared network ```runverter``` and exposes port 80 and 443 to the outside world. Port 8080 is exposed to the docker machine to make the haproxy webinterface reachable via SSH tunnel. The local folder ```/etc/haproxy/certs/``` will be available inside the container to access required ssl certificates.

## Build the image 

```shell
$ docker build --tag stefankracht/runverter-haproxy .
```

## Publish the image

```shell
$ docker push stefankracht/runverter-haproxy
```
