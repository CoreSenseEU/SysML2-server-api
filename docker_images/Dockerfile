#Dockerfile obtained from https://github.com/gorenje/sysmlv2-jupyter-docker

FROM openjdk:11-slim

##
## This Dockerfile setups the SysML v2 API server and is designed to be
## run in conjunction with the Jupyter server. This dockerfile is specifically
## designed to be run with docker compose.
##

RUN apt-get --quiet --yes update && apt-get install -yqq wget

##
## Use a non-root user for installation
##
ARG NB_USER=sysml
ARG NB_UID=1000
ENV USER ${NB_USER}
ENV NB_UID ${NB_UID}
ENV HOME /home/${NB_USER}

RUN adduser --disabled-password \
    --gecos "Default user" \
    --uid ${NB_UID} \
    ${NB_USER}

USER root
RUN chown -R ${NB_UID} ${HOME}

## Switch to the lowly user, no more root.
USER ${NB_USER}
WORKDIR ${HOME}

ARG SBT_VERSION=1.2.8
RUN wget -q https://github.com/sbt/sbt/releases/download/v${SBT_VERSION}/sbt-${SBT_VERSION}.tgz

## Defining the RELEASE down here ensures that the previous comamnds can
## be recycled since they're not affected by the release version.
ARG RELEASE=2023-02

RUN wget -q https://github.com/Systems-Modeling/SysML-v2-API-Services/archive/${RELEASE}.tar.gz?ts=20220819Z020300+00 -O ${RELEASE}.tar.gz

RUN tar xfz sbt-${SBT_VERSION}.tgz
RUN tar xfz ${RELEASE}.tar.gz

WORKDIR ${HOME}/SysML-v2-API-Services-${RELEASE}

##
## Basically this is all taken from
##     https://github.com/Systems-Modeling/SysML-v2-API-Services
## and the installation instructions over there.
##
RUN ${HOME}/sbt/bin/sbt clean
RUN ${HOME}/sbt/bin/sbt update
RUN ${HOME}/sbt/bin/sbt compile

## Play framework complains about the secret key length...
RUN sed s/key=.whatever./key=\"longersecretnowarnings\"/ -i conf/application.conf

## need to allow the jupyter server access to the API server
RUN echo "\n\nplay.filters.hosts {\n   allowed = [\"sysmlapiserver:9000\", \"localhost:9000\"]\n}\n" >> conf/application.conf

## point the image to the postgres that is setup via docker compose
RUN sed s/localhost:5432/postgresdbserver:5432/ -i conf/META-INF/persistence.xml

## don't recreate the database if it already exists.
RUN sed s/value=.create-drop./value=\"update\"/ -i conf/META-INF/persistence.xml

EXPOSE 9000
ENTRYPOINT ${HOME}/sbt/bin/sbt run


