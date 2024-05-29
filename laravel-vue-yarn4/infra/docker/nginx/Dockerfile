FROM nginx:1.20-alpine
SHELL ["/bin/ash", "-oeux", "pipefail", "-c"]

ENV TZ=UTC

RUN apk update && \
  apk add --update --no-cache --virtual=.build-dependencies g++

# nginx config file
COPY ./infra/docker/nginx/*.conf /etc/nginx/conf.d/
# COPY ./infra/docker/nginx/default.conf /etc/nginx/templates/default.conf.template
# COPY ./infra/docker/nginx/nginx.conf /etc/nginx/nginx.conf
## イメージのビルド時に、アプリケーションのコードがコピーされるようにする
# COPY ./src/public /work/src/public

WORKDIR /work/src
