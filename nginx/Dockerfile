# NGINX image with build artifact

FROM nginx:latest

LABEL MAINTAINER="goanpao <sav.extra@gmail.com>"
ENV REFRESHED_AT 2018-03-25

ENV GITHUB_REPO https://github.com/goanpao/sh-spring-music/releases/download
ENV RELEASE v1.0.0
ENV STATIC_FILE spring-music-static.zip

RUN apt-get update -qq
RUN apt-get install -qqy curl wget unzip nano
RUN apt-get clean
RUN wget -O /tmp/${STATIC_FILE} ${GITHUB_REPO}/${RELEASE}/${STATIC_FILE}
RUN unzip /tmp/${STATIC_FILE} -d /usr/share/nginx/assets/

COPY default.conf /etc/nginx/conf.d/default.conf

# tweak nginx image set-up, remove log symlinks
RUN rm /var/log/nginx/access.log /var/log/nginx/error.log

# install Filebeat
ENV FILEBEAT_VERSION=filebeat_1.2.3_amd64.deb
RUN curl -L -O https://download.elastic.co/beats/filebeat/${FILEBEAT_VERSION} \
 && dpkg -i ${FILEBEAT_VERSION} \
 && rm ${FILEBEAT_VERSION}

# configure Filebeat
ADD filebeat.yml /etc/filebeat/filebeat.yml

# CA cert
RUN mkdir -p /etc/pki/tls/certs
ADD logstash-beats.crt /etc/pki/tls/certs/logstash-beats.crt

# start Filebeat
ADD ./start.sh /usr/local/bin/start.sh
RUN chmod +x /usr/local/bin/start.sh
CMD [ "/usr/local/bin/start.sh" ]
