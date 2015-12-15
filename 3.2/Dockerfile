# a build context 
# ubuntu üzerine mongodb kurulumunu yapılan image'ımız
FROM ubuntu:latest
MAINTAINER Hakan Ozler <ozler.hakan@gmail.com>

# gerekli ortam değişkenleri atamasını yap
ENV MONGO_VERSION 3.2.0
ENV MONGO_DIR /data
ENV CONFIG_PATH /home/kodcu/mongod
ENV KEY_PATH https://dl.dropboxusercontent.com/u/15056258/mongodb.keyfile

# kodcu isminde grup ve kullanıcı ekle, gerekli dizinleri oluştur
RUN groupadd -r kodcu && useradd -rm -g kodcu kodcu
RUN mkdir -p $CONFIG_PATH \ 
	&& mkdir -p $MONGO_DIR/db \
	&& mkdir -p $MONGO_DIR/log 

# mongodb yükleniyor ve diğer önemli komutlar tek bir atımda işleniyor
RUN set -x \ 
	&& apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927 \
	&& echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list \
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

# container'ın kendisi 27017 portunu dinlemeye başlar, host'un görmesi için -P ya da -p kullan  
EXPOSE 27017
USER kodcu

# bind mount volume: host'taki config dosyasını container içindeki volume noktasına taşıma yap
ENTRYPOINT ["mongod", "-f", "/home/kodcu/mongod/mongod.conf"]
