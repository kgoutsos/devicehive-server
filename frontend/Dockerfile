FROM kgoutsos/devicehive-server-base

MAINTAINER kgoutsos

RUN mv /opt/devicehive/devicehive-frontend/target/devicehive-frontend-${DH_VERSION}-boot.jar /opt/devicehive/

ADD devicehive-start.sh /opt/devicehive/

VOLUME ["/var/log/devicehive"]

WORKDIR /opt/devicehive/

ENTRYPOINT ["/bin/sh"]

CMD ["./devicehive-start.sh"]

EXPOSE 8080
