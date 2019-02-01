FROM ubuntu:16.04
LABEL maintainer="exadatum_"


# Arguments and Environment Variables
ARG CDH_REPO_URI=https://archive.cloudera.com/cdh6/6.1.0/ubuntu1604/apt
ARG CDH_RELEASE_NAME=xenial-cdh6.1.0
ENV MOUNT_POINT=/root/mount
ENV ABFS_URI=


# Setting up required packages on ubuntu
RUN apt-get update -yqq \
 && apt-get install apt-utils -y \
 && apt-get install software-properties-common -yqq \
 && add-apt-repository -y ppa:webupd8team/java \
 && apt-get update -y \
 && echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections \
 && apt-get install -y oracle-java8-installer \
 && apt-get install apt-transport-https -yqq \
 && apt-get install module-init-tools -yqq \
 && apt-get install module-assistant -yqq \
 && apt-get install --reinstall linux-image-`uname -r` -yqq \
 && apt-get install wget -yqq


# Configure cloudera repository
RUN echo "deb ${CDH_REPO_URI} ${CDH_RELEASE_NAME} contrib" >> /etc/apt/sources.list.d/cloudera.list \
 && wget ${CDH_REPO_URI}/archive.key \
 && apt-key add archive.key \
 && apt-get update -yqq \
 && apt-get install hadoop hadoop-hdfs libhdfs0 openssl hadoop-hdfs-fuse -yqq

# Steps done in one RUN layer:
# - Install packages
# - OpenSSH needs /var/run/sshd to run
# - Remove generic host keys, entrypoint generates unique keys
RUN apt-get update && \
    apt-get -y install openssh-server && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /var/run/sshd && \
    rm -f /etc/ssh/ssh_host_*key*

COPY files/sshd_config /etc/ssh/sshd_config
COPY files/scripts/create-sftp-user /usr/local/bin/
COPY files/scripts/entrypoint.sh /
COPY files/sshd /etc/pam.d/sshd

RUN chmod +x /entrypoint.sh

EXPOSE 22

ENTRYPOINT ["/entrypoint.sh"]

