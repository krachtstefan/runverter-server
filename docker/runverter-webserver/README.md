# runverter-webserver

This container serves the frontend of the [runverter ember app](https://github.com/krachtstefan/runverter).  It shows the value of the ```runverter:current``` Redis key which holds the content of the index page that was set with [ember-cli-deploy](https://github.com/ember-cli/ember-cli-deploy). If a get param ```index_key``` is provided, it will lookup the requested version in redis.

## Run container

```shell
$ docker run --name runverter -d -p 80:3000 stefankracht/runverter-webserver
```

The exposed port may be changed depending on loadbalancer/proxy setup of the server/cluster.

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
