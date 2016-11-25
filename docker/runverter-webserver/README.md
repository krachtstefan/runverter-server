# runverter-webserver

This container serves the frontend of the [runverter ember app](https://github.com/krachtstefan/runverter).  

When the root path ```/``` is requested, it shows the value of the ```runverter:index:current-content``` Redis key which holds the content of the index page that was set with [ember-cli-deploy](https://github.com/ember-cli/ember-cli-deploy). If a get parameter ```index_key``` is provided, it will lookup the requested version in redis.

All requests other than ```/``` will be forwarded to the ```runverter``` forder to look for static files, so be sure to mount a volume like ```-v ~/runverter:/usr/src/app/runverter```. This is required to serve files that are not allowed to be deliverd via CDN like ```/robots.txt``` or ```/manifest.appcache```. They are expected to be in the root directory of the same origin.

## Start container

```shell
$ docker run -d --name runverter-app1 --net -v ~/runverter:/usr/src/app/runverter runverter stefankracht/runverter-webserver
```

This examples starts one instance of the node cluster. It exposes the port 3000 in the shared network ```runverter``` and its ip will become available via the hostname ```runverter-app1```. You can start multiple instances like ```runverter-app2```, ```runverter-app2``` etc.

## Environment variables

The container assumes a running redis instance on ```runverter-redis:6379``` and the keys should start with ```runverter```. You can overwrite the environment variables by adding ```--env VARIABLE=new_value``` to the ```docker run``` command.

### `APP_NAME`

The name of the application as deployed in Redis. This is ```runverter``` by default.

### `REDIS_HOST`

The hostname of the Redis server. Defaults to ```runverter-redis```.

### `REDIS_PORT`

The port that Redis is listening on. Only necessary if redis is not listening to the default port ```6379```.

### `REDIS_SECRET`

The shared secret to use for authenticating to Redis. It is blank by default,
which disables authentication.

## Build the image 

```shell
$ docker build --tag stefankracht/runverter-webserver .
```

## Publish the image

```shell
$ docker push stefankracht/runverter-webserver
```
