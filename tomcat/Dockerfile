# Apache Tomcat image with build artifact

FROM tomcat:8.5.4-jre8

LABEL MAINTAINER="goanpao <sav.extra@gmail.com>"
ENV REFRESHED_AT 2018-03-25

ENV GITHUB_REPO https://github.com/goanpao/sh-spring-music/releases/download
ENV RELEASE v1.0.0
ENV APP_FILE spring-music.war
ENV TERM xterm
ENV JAVA_OPTS -Djava.security.egd=file:/dev/./urandom

RUN apt-get update -qq
RUN apt-get install -qqy curl wget
RUN apt-get clean
RUN touch /var/log/spring-music.log
RUN chmod 666 /var/log/spring-music.log
RUN wget -q -O /usr/local/tomcat/webapps/ROOT.war ${GITHUB_REPO}/${RELEASE}/${APP_FILE}
RUN mv /usr/local/tomcat/webapps/ROOT /usr/local/tomcat/webapps/_ROOT

COPY tomcat-users.xml /usr/local/tomcat/conf/tomcat-users.xml

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
