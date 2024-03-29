ARG BASE="ubuntu:18.04"
ARG ARCH="x64"

FROM $BASE
ARG ARCH

# install build tools and dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        wget \
    && rm -rf /var/lib/apt/lists/* 

WORKDIR /opt 
 
RUN wget --show-progress --progress=bar:force -O node.tar.gz https://nodejs.org/dist/v10.24.1/node-v10.24.1-linux-${ARCH}.tar.gz && \
    tar -C /usr/local --strip-components 1 -xzf  node.tar.gz && \
    rm node.tar.gz 

RUN npm -g install -y http-server 

# default website location 
RUN mkdir -p /var/httpd/www
WORKDIR /var/httpd/www
COPY ./index.html .

# support non-root user
RUN useradd --user-group --system httpd
EXPOSE 8080

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["http-server"]
