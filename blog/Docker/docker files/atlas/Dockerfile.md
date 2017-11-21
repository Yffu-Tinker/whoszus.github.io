#### 360 atlas mysql-proxy \ Dockerfile index 

```shell
FROM debian:jessie-slim

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN groupadd -r gosuncn && useradd -r -g gosuncn gosuncn

ENV GOSU_VERSION 1.10
RUN set -ex; \ 
	apt-get install -y --no-install-recommends wget 
	
ENV ATLAS_DOWNLOAD_URL https://github.com/Qihoo360/Atlas/releases/download/2.1/Atlas-2.1-debian7.0-x86_64.deb
RUN set -ex; \
	\
	buildDeps=' \
		wget \
	'; \
	apt-get update; \
	rm -rf /var/lib/apt/lists/*; \
	\
	wget -O Atlas-2.1-debian7.0-x86_64.deb "$ATLAS_DOWNLOAD_URL"; \
	wget -O /usr/lib/libmysqlclient.so.18 http://files.directadmin.com/services/es_7.0_64/libmysqlclient.so.18
	dpkg -i Atlas-2.1-debian7.0-x86_64.deb
	\
RUN mkdir /data && chown gosuncn:gosuncn /data
VOLUME /data
WORKDIR /data

COPY docker-entrypoint.sh /usr/local/bin/
COPY atlas.cnf /usr/local/mysql-proxy/test.cnf
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 1234
CMD ["/usr/local/mysql-proxy/bin/mysql-proxyd test start"]
```

