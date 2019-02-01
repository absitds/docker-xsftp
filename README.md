# Docker-xsftp with Adls Gen2 Fuse

This repository contains **Dockerfile** for internal SFTP docker with adls fuse mounted.

## Informations

* Based on Ubuntu official Image [ubuntu:16.04](https://hub.docker.com/_/ubuntu) 
* Install [Docker](https://www.docker.com/)
* Install [Docker Compose](https://docs.docker.com/compose/install/)


## Build

For Building the docker image for xsftp you first need to build the docker image using the dockerfile added in the repository.For adding [Extra Packages](https://airflow.incubator.apache.org/installation.html#extra-package) edit the docker file and then build.

    docker build -t xsftp:1.0.0  .

## Usage

Run the container using docker run 

    docker run -p 2222:22 -v /home/exa00015/mount/conf:/etc/hadoop/conf -e MOUNT_POINT=/home/abssftp/mount -e ABFS_URI=abfs://xana-sprk-itds-dev-wus@absitdsdevwus001.dfs.core.windows.net -d --privileged xsftp:1.0.0 abssftp:pass:::mount

# Issues

- Issue Tracker: https://github.com/absitds/docker-xsftp/issues

