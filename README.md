# Движок блога Эгея в Docker-контейнере.
### Связка Caddy + PHP7 + MariaDB. https от [Let's Encrypt](https://letsencrypt.org/) из коробки.

_В проекте используется, на мой взгляд самый прогрессивный веб-сервер на сегодня - [Caddy](https://caddyserver.com/). Его очень легко конфигурировать и он поддерживает http/2 и автоматический https из коробки._

1. Склонируйте на сервер или на свой компьютер этот репозиторий.

2. Скачайте архив с [последней версией Эгеи](http://blogengine.ru/get/).

3. Распакуйте содержимое архива в папку `blog` внутри текущего проекта.

4. Скопируйте файл `.env.dist` в файл `.env` и установите там пароли для root и вашего пользователя MySQL.

5. Для развертывания Эгеи на компьютере разработчика скопируйте файл ``caddy/Caddyfile.dev`` в файл ``caddy/Caddyfile``.

6. Для развертывания Эгеи на боевом сервере скопируйте файл ``caddy/Caddyfile.prod`` в файл ``caddy/Caddyfile``, и измените в этом файле название доменов для блога и Adminer, а так же e-mail, на который будет регистрироваться Let's Encrypt. В файле `docker-compose.yaml` в секции конфигурации caddy оставьте только 80 и 443 порты. Порт 2015 должен быть открыт всегда (и на dev и на prod).

7. В  `blog/user/config.php` нужно прописать: `$_config['url_composition'] = 'synthetic';` и после этого сбросить кеш `/?go=@sync/`

8. Для сборки проекта на компьютере разработчика наберите в консоли `docker-compose build`. Эту команду необходимо выполнять каждый раз, если вы что-то меняете в конфигурационных файлах Докера. Для сборки проекта на продакшене используйте команду ``docker-compose -f docker-compose.prod.yaml build``, указывающую на production-версию конфигурационного файла для Docker.

9. На компьютере разработчика запускать командой `docker-compose up`.

10. На боевом сервере запускать командой `docker-compose -f docker-compose.prod.yaml up -d`. Ключ `-d` указывает, на то, что контейнеры надо запустить в режиме демонов, а ключ `-f` указывает, что для запуска нужно использовать production-версию конфигурации. В случае рестарта сервера контейнеры будут запускаться автоматически. Для остановки контейнеров надо выполнить команду `docker-compose -f docker-compose.prod.yaml down`.
