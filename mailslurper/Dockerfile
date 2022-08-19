FROM ubuntu:22.04

RUN mkdir -p /opt/mailslurper

COPY ./mailslurper-1.14.1-linux /opt/mailslurper

WORKDIR /opt/mailslurper

ENTRYPOINT ["/opt/mailslurper/mailslurper"]
