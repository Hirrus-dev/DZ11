FROM jenkins/jenkins:alpine

USER root
RUN apk update && apk add docker && apk add shadow
RUN usermod -aG docker jenkins