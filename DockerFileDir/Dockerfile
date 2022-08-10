FROM centos:latest
MAINTAINER Hammer Head
LABEL Description="Golang and SSHD installed on this baby"

# Use ENV to configure the environment for they OS during the build process - wont persist afterwards
#ENV GOROOT=/usr/local/go \
#    GOPATH=/home/go_user/go \
#    GOBIN=/home/go_user/go/bin \
#    PATH="/user/local/go/bin:$PATH"

# Install GOLANG and other useful software
RUN yum -y update \
    && yum install -y curl wget which iproute \
    && GOBINARY=$(curl https://golang.org/VERSION?m=text).linux-amd64.tar.gz \
    && wget "https://dl.google.com/go/$GOBINARY" \
    && tar xvf $GOBINARY --directory /usr/local \
    && chmod 755 /usr/local/go/bin/go \
    && rm -rf $GOBINARY

# create user called golang to sudo into and run go programs with
ARG  GOUSER_HOME=/home/golang
ARG  GOUSER=golang
ARG  GOGUID=1000

# Create user account set password and add user to sudoers wheel group
RUN groupadd -g ${GOGUID} ${GOUSER} \
    && useradd -m -u -g ${GOGUID} -g ${GOUSER} -s /bin/bash ${GOUSER}

# Copy custom .bashrc file, updated with GOLANG paths to users home dir
COPY bashrc ${GOUSER_HOME}/.bashrc
RUN chown ${GOUSER}:${GOUSER} ${GOUSER_HOME}/.bashrc && chmod 0644 ${GOUSER_HOME}/.bashrc
