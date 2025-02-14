FROM phpswoole/swoole:php8.4-alpine

ENV MYSQLDEF_VERSION=v0.17.28
ENV SQLITE3DEF_VERSION=v0.17.28

WORKDIR /tmp

RUN apk add --no-cache make npm && \
    npm install -g chokidar && \
    docker-php-ext-install pcntl pdo_mysql && \
    curl -LO https://github.com/sqldef/sqldef/releases/download/${MYSQLDEF_VERSION}/mysqldef_linux_amd64.tar.gz && \
    tar xvf mysqldef_linux_amd64.tar.gz && \
    mv ./mysqldef /usr/bin/mysqldef && \
    rm ./mysqldef_linux_amd64.tar.gz

ENV NODE_PATH=/usr/local/lib/node_modules

WORKDIR /app
