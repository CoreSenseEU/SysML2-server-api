# SysML2-server-api
Dockerized example for running a Local SysML2 server and API pipeline with some examples 

# Setup Instructions 

## 0. Prerequisites
The following instuctions are extracted from the official repo https://github.com/Systems-Modeling/SysML-v2-API-Services
and the docker API Server Dockefile has been extracted from https://github.com/gorenje/sysmlv2-jupyter-docker 

1. Install docker following https://docs.docker.com/desktop/install/ubuntu/ Instructions
> If using Linux I recommend configuring this -> https://docs.docker.com/engine/install/linux-postinstall/ (only things related with non root-user)

## 1. Clone repo
```
git clone https://github.com/miferco97/SysML2-server-api 

cd SysML2-server-api

```

## 2. Clone postgres DB 

```
$ docker pull postgres
```

## 3. Build docker image

```
$ docker build ./docker_images -t sysml-v2-api # --progress=plain for verbose stdout
```

## 4. Create Volume for store database
```
$ docker volume create postgresdbserver
```

## 4. Run docker compose 

```
$ docker compose up # add -d for detaching 
```
The server waits until the first request is done for setup. After this it can spend up to 2 minutes in initializing it completely (the first execution even more)
When the server is ready this prompt will be displayed:
~~~
[info] play.api.Play - Application started (Dev) (no global state)
~~~



## 5. Test 

go to http://localhost:9000/docs/ inside your web browser to check if all works (it can spend up to 2 minutes in initailizing the server)

For futher examples using the API through Jupyter Notebooks refer https://github.com/Systems-Modeling/SysML-v2-API-Cookbook


## 6. Stop execution

For closing the compose when running in detach mode:

```
$ docker compose down 
```







