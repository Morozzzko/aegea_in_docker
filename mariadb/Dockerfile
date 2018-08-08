FROM mariadb:latest
LABEL maintainer "Vladimir Hodakov <vladimir@hodakov.me>"

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone