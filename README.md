# Simple Demo for Docker & Kubernetes

## Prerequisites

* Maven
* Java
* Docker
* Kubernetes
* Curl

Install and run Docker & Kubernetes locally (Linux/Windows/Mac):
https://docs.docker.com/get-docker/

## Run app on your OS

REST endpoints:
* GET localhost:8080/actuator/health: health endpoint
* GET localhost:8080/api/ping: get PONG message

Get source code, compile app and run app:

```sh
> git clone https://github.com/pmgysel/demo-docker-kubernetes.git
> cd demo-docker-kubernetes
> mvn clean install
> java -jar target/demo-docker-kubernetes-0.0.1-SNAPSHOT.jar
```

Test running app in second terminal:

```sh
> curl -X GET localhost:8080/api/ping

Hello world, here is demo-docker-kubernetes version 0.0.1-SNAPSHOT
```

## Docker demo

Option 1: Create Docker image from JAR

```sh
> docker build -t demo-docker-kubernetes:0.0.1 -f Dockerfile.from.artifact .
```

Option 2: Create Docker image from source

```sh
> docker build -t demo-docker-kubernetes:0.0.2 -f Dockerfile.from.source .
```

... then run the image inside a Docker container:

```sh
> docker run -p 8080:8080 demo-docker-kubernetes:0.0.1
> curl -X GET localhost:8080/api/ping

Hello world, here is demo-docker-kubernetes version 0.0.1-SNAPSHOT
```

Clean up everything:

```sh
> docker ps
CONTAINER ID     IMAGE                          COMMAND                  CREATED  ....
af2b2f9d190c     demo-docker-kubernetes:0.0.1   "/bin/sh -c 'java -j…"   8 seconds ...
> docker stop af2b2f9d190c
```

## Kubernetes demo

```sh
> cd kubernetes
> kubectl apply -f deployment.yaml
> kubectl get deployments
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
demo-deployment   1/1     1            1           11s
> kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
demo-deployment-5ffc866f6-4ffsg   1/1     Running   0          31s
> kubectl apply -f service.yaml
> kubectl get services
NAME           TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
demo-service   NodePort    10.98.86.21   <none>        8080:30001/TCP   19s
> curl -X GET http://localhost:30001/api/ping

Hello world, here is demo-docker-kubernetes version 0.0.1-SNAPSHOT
```

Finally, clean up running pods and services:

```sh
> kubectl delete deployment demo-deployment
> kubectl delete service demo-service
```
