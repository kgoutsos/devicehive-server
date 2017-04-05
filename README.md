# Installation
The [DeviceHive](https://github.com/devicehive/devicehive-java-server) Docker container accepts the following environment variables to enable persistent storage in PostgreSQL, message bus support through Apache Kafka and scalable storage of device messages using Apache Cassandra.

## Configuration
### PostgreSQL
* ```${DH_POSTGRES_ADDRESS}``` — Address of PostgreSQL server instance.
* ```${DH_POSTGRES_PORT}``` — Port of PostgreSQL server instance. Default: 5432. Ignored if ```${DH_POSTGRES_ADDRESS}``` is undefined.
* ```${DH_POSTGRES_DB}``` — PostgreSQL database name for DeviceHive metadata. It is assumed that it already exists and is either blank or has been initialized by DeviceHive. Ignored if ```${DH_POSTGRES_ADDRESS}``` is undefined.
* ```${DH_POSTGRES_USERNAME}``` and ```${DH_POSTGRES_PASSWORD}``` — login/password for DeviceHive user in PostgreSQL that have full access to ```${DH_POSTGRES_DB}```. Ignored if  ```${DH_POSTGRES_ADDRESS}``` is undefined.

### Kafka
To enable DeviceHive to use the Apache Kafka message bus define the following environment variables:
* ```${DH_KAFKA_ADDRESS}``` — Address of the Apache Kafka broker node. If no address is defined DeviceHive will run in standalone mode.
* ```${DH_KAFKA_PORT}``` — Port of Apache Kafka broker node. Ignored if ```${DH_KAFKA_ADDRESS}``` is undefined.
* ```${DK_ZH_ADDRESS}``` — Comma-separated list of addresses of ZooKeeper instances. Ignored if ```${DH_KAFKA_ADDRESS}``` is undefined.
* ```${DK_ZK_PORT}``` — Port of ZooKeeper instances. Ignored if ```${DH_KAFKA_ADDRESS}``` is undefined.
* ```${DH_RPC_SERVER_REQ_CONS_THREADS}``` — Kafka request consumer threads, defaults to ```1```.
* ```${DH_RPC_SERVER_WORKER_THREADS}``` — Server worker threads, defaults to ```1```.
* ```${DH_RPC_SERVER_DISR_WAIT_STRATEGY}``` — Disruptor wait strategy, defaults to ```blocking```. Available options are: ```sleeping```, ```yielding```, ```busy-spin```.
* ```${DH_RPC_CLIENT_RES_CONS_THREADS}``` — Kafka response consumer threads, defaults to ```1```.

More configurable parameters at [devicehive-start.sh](backend/devicehive-start.sh) and [devicehive-start.sh](frontend/devicehive-start.sh).

## Build
The project consists of three images: the base Java image, the DeviceHive Backend image, and the DeviceHive Frontend image. The base image is used to avoid duplication of the work required to setup Maven and then compile the DeviceHive app.

By default, the Dockerfiles for the backend and the frontend are set to use the base image [kgoutsos/devicehive-server-base](base). As usual, Docker will look for it locally and if it's not there it will be pulled from the Docker hub. You can build the image locally with `docker build -t kgoutsos/devicehive-server-base base`. The name of the image can be changed to any arbitrary name as long as the Dockerfiles ([Backend](backend/devicehive-start.sh) and [Frontend](frontend/devicehive-start.sh)) are updated accordingly.

The backend and frontend images can be built with `docker build -t devicehive-backend backend` and `docker build -t devicehive-frontend frontend` respectively.

## Run
In order to run DeviceHive from the Docker container, define environment variables as per your requirements and run:
```
docker-compose up
```
You can access your DeviceHive API http://devicehive-host-url/api.

Default JWT token: ```eyJhbGciOiJIUzI1NiJ9.eyJwYXlsb2FkIjp7InVzZXJJZCI6MSwiYWN0aW9ucyI6WyIqIl0sIm5ldHdvcmtJZHMiOlsiKiJdLCJkZXZpY2VHdWlkcyI6WyIqIl0sImV4cGlyYXRpb24iOjE0OTcwMTg5NDgyODcsInRva2VuVHlwZSI6IkFDQ0VTUyJ9fQ.BQgno2Tz233VG530JUK6zlCVrAdeny1IvKQRswd6hVw```

Refresh Token:
```eyJhbGciOiJIUzI1NiJ9.eyJwYXlsb2FkIjp7InVzZXJJZCI6MSwiYWN0aW9ucyI6WyIqIl0sIm5ldHdvcmtJZHMiOlsiKiJdLCJkZXZpY2VHdWlkcyI6WyIqIl0sImV4cGlyYXRpb24iOjE0OTcwMTg5NDgzMTgsInRva2VuVHlwZSI6IlJFRlJFU0gifX0.s0UHB1L5zXwdrh4xIUjBAc_y4oWfTK8ixdLh0guNf60```

## Logging
By default DeviceHive writes minimum logs for better performance. You can see the default settings at [logback.xml](https://github.com/devicehive/devicehive-java-server/blob/development/src/main/resources/logback.xml).
It is possible to override logging without rebuilding the jar file or the Docker image. Given you have log config `config.xml` in the current folder include the following parameters:
```
docker run -p 80:80 -v ./config.xml:/opt/devicehive/config.xml -e _JAVA_OPTIONS="-Dlogging.config=file:/opt/devicehive/config.xml" devicehive/devicehive
```
