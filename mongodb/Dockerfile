# MongoDB image with host-based data volume

FROM mongo:latest
LABEL MAINTAINER="goanpao <sav.extra@gmail.com>"
ENV REFRESHED_AT 2018-03-25

VOLUME ["/data/db"]
WORKDIR /data
CMD ["mongod", "--smallfiles"]
