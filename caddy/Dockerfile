FROM alpine:3.5
LABEL maintainer "Pavel Pavlenko <region23@gmail.com>"

LABEL caddy_version="0.11.0" architecture="amd64"

ARG plugins=http.ipfilter

RUN apk add --no-cache openssh-client git tar curl

# Устанавливаем пакет с таймзонами для корректной работы time.LoadLocation в Go
RUN apk update && apk upgrade && apk add -U tzdata && apk add ca-certificates

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN mkdir -p /root/.caddy/

RUN curl --silent --show-error --fail --location \
      --header "Accept: application/tar+gzip, application/x-gzip, application/octet-stream" -o - \
      "https://caddyserver.com/download/linux/amd64?license=personal&telemetry=off&plugins=${plugins}" \
    | tar --no-same-owner -C /usr/bin/ -xz caddy \
 && chmod 0755 /usr/bin/caddy \
 && /usr/bin/caddy -version

EXPOSE 80 443 2015
VOLUME /root/.caddy
RUN mkdir -p /var/www/blog
WORKDIR /var/www/blog

COPY Caddyfile /etc/Caddyfile

ENTRYPOINT ["/usr/bin/caddy"]
CMD ["--conf", "/etc/Caddyfile", "--log", "stdout", "--agree"]
