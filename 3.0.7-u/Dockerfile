FROM ubuntu:latest
MAINTAINER Hakan Ozler <ozler.hakan@gmail.com>

ENV MONGO_VERSION 3.0.7
ENV MONGO_DIR /data
ENV CONFIG_PATH /home/kodcu/mongod
ENV KEY_PATH https://dl.dropboxusercontent.com/u/15056258/mongodb.keyfile

RUN groupadd -r kodcu && useradd -rm -g kodcu kodcu
RUN mkdir -p $CONFIG_PATH \ 
	&& mkdir -p $MONGO_DIR/db \
	&& mkdir -p $MONGO_DIR/log 

RUN set -x \ 
	&& apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 \
	&& echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.0.list \
	&& apt-get update \ 
	&& apt-get install -y \ 
				mongodb-org=$MONGO_VERSION \
				mongodb-org-server=$MONGO_VERSION \
				mongodb-org-shell=$MONGO_VERSION \
				mongodb-org-mongos=$MONGO_VERSION \
				mongodb-org-tools=$MONGO_VERSION \
				curl \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/* \
	&& rm -rf /var/lib/mongodb \
	&& curl -SLk $KEY_PATH -o mongod.keyfile \
	&& mv mongod.keyfile $MONGO_DIR/log/mongod.keyfile \
	&& chmod 600 $MONGO_DIR/log/mongod.keyfile \
	&& chown -R kodcu:kodcu $MONGO_DIR

EXPOSE 27017
USER kodcu
ENTRYPOINT ["mongod", "-f", "/home/kodcu/mongod/mongod.conf"]
