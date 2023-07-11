# SysML2-server-api
Dockerized example for running a Local SysML2 server and API pipeline with some examples 

# Setup Instructions 

The following instuctions are extracted from https://github.com/Systems-Modeling/SysML-v2-API-Services

1. Install docker following https://docs.docker.com/desktop/install/ubuntu/ Instructions
>> If using Linux I recommend configuring this -> https://docs.docker.com/engine/install/linux-postinstall/ (only things related with non root-user)

# 2. Clone postgres DB 

$ docker pull postgres

# 3. Build docker image
$ cd docker_images && docker build . -t sysml-v2-api --progress=plain

# 4. Run docker compose example

FROM the REPO path $ run cd .. if you are inside docker_images folder

$ docker compose up 

# 5. test 

go to localhost:9000/docs/ inside your web browser to check if all works






