FROM frolvlad/alpine-oraclejdk8

MAINTAINER kgoutsos

ENV DH_VERSION="3.0.0-SNAPSHOT"
ENV MAVEN_VERSION="3.3.3"

RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh curl && \
    mkdir -p /opt/devicehive && \
    cd /usr/share && \
    wget http://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz -O - | tar xzf - && \
    mv /usr/share/apache-maven-$MAVEN_VERSION /usr/share/maven && \
    ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven

RUN git clone -b development https://github.com/devicehive/devicehive-java-server.git /opt/devicehive && \
    cd /opt/devicehive && \
    mvn clean package -Pbooted-rdbms,shim-kafka,!booted-riak -DskipTests
