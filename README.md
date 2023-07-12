# SysML2-server-api
Dockerized example for running a Local SysML2 server and API pipeline with some examples 

# Setup Instructions 

The following instuctions are extracted from the official repo https://github.com/Systems-Modeling/SysML-v2-API-Services
and the docker API Server Dockefile has been extracted from https://github.com/gorenje/sysmlv2-jupyter-docker 

## 0. Prerequisites

For using this Server API docker is needed. For install docker follow https://docs.docker.com/desktop/install/ubuntu/ installing instructions.
> If using Linux I recommend configuring this -> https://docs.docker.com/engine/install/linux-postinstall/ (only things related with non root-user)

## 1. Clone repo
```
git clone https://github.com/miferco97/SysML2-server-api 
cd SysML2-server-api
```

## 2. Pull postgres image

For downloading the postgres docker image run:

```
docker pull postgres
```

## 3. Build API-Server docker image

Then the SysML2 API Server image has to be build. For building it run:

```
$ docker build ./docker_images -t sysml-v2-api # --progress=plain for verbose stdout
```

## 4. Create Volume for store database
In order to store the database in a persistent way a docker volume has to be created:

```
$ docker volume create postgresdbserver
```

For deleting it you can run:

```
$ docker volume rm postgresdbserver
``` 

# Run the SysML2 API Server 

For running the SysML2 API Server a docker compose is used for starting all the containers toguether:
```
$ docker compose up # add -d for detaching 
```

The server waits until the first request is done for setup. After this it can spend up to 2 minutes in initializing it completely (the first execution even more)
When the server is ready this prompt will be displayed:
~~~
[info] play.api.Play - Application started (Dev) (no global state)
~~~

# Testing the API 

go to http://localhost:9000/docs/ inside your web browser to check if all works (it can spend up to 2 minutes in initailizing the server)

For futher examples using the API through Jupyter Notebooks refer https://github.com/Systems-Modeling/SysML-v2-API-Cookbook


# Stop execution

For closing the compose when running in detach mode:

```
$ docker compose down 
```







