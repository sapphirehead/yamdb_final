# yamdb_final

![yamdb workflow](https://github.com/sapphirehead/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## *Проект YaMDb*

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Category) может быть расширен администратором (например, можно добавить категорию «Картины» или «Ювелирка»).

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

### Используемые технологии:

Django 2.2.16

Python 3.10.4

Django REST Framework 3.12.4

Simple-JWT 4.8.0

PostgreSQL 13.0-alpine

Nginx 1.21.3-alpine

Gunicorn 20.0.4

Docker 20.10.17, build 100c701

Docker-compose 3.8

GitHub Actions

### Как запустить проект:

- Для развёртывания проекта необходимо скачать его в нужную вам директорию, например:

```git clone git@github.com:sapphirehead/yamdb_final.git```

*Нужно установить docker & docker-compose. Настроить Dockerfile, docker-compose.yaml, yamdb_workflow.yml согласно вашим данным.*
*После настроек и push на GitHub проект проверятся тестами и линтером flake8, загружает образ на Docker Hub, разворачивает образ на сервере.*

- В директории infra создайте файл .env с переменными окружения для работы с базой данных:

```
DJANGO_KEY='your Django secret key'
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

- Для запуска необходимо выполнить из директории с проектом команду:

```sudo docker-compose up -d```

_Для пересборки команда up выполняется с параметром --build_

```sudo docker-compose up -d --build```

### На сервере.

- Сделать миграции:

```sudo docker-compose exec web python manage.py migrate```

- Создать суперпользователя:

```sudo docker-compose exec web python manage.py createsuperuser```

- Собрать статику:

```sudo docker-compose exec web python manage.py collectstatic --no-input```

- Вы также можете создать дамп (резервную копию) базы:

```sudo docker-compose exec web python manage.py dumpdata > fixtures.json```

- или, разместив, например, файл fixtures.json в папке с Dockerfile, загрузить в базу данные из дампа:

```sudo docker-compose exec web python manage.py loaddata fixtures.json```

### Некоторые полезные команды:

- Локально создать образ с нужным названием и тегом:

```docker build -t <username>/<imagename>:<tag> .```

- Авторизоваться через консоль:
```docker login```
- А можно сразу указать имя пользователя
```docker login -u <username>```
- Загрузить образ на DockerHub:
```docker push <username>/<imagename>:<tag>```

- Проверить файлы в корне проекта:

```sudo docker-compose exec web ls -a```

- Остановка всех контейнеров:

```sudo docker-compose down```

- Мониторинг запущенных контейнеров:

```sudo docker stats```

- Команда покажет, сколько места на диске занимают образы, контейнеры, тома и билд-кеш.

```sudo docker system df```


- Останавливаем и удаляем контейнеры, сети, тома вместе со всеми зависимостями. Осталются только образы:

```sudo docker-compose down -v```

- Остановить проект сохранив данные в БД:

```docker-compose down```

- Остановить проект удалив данные в БД:

```docker-compose down --volumes```

- Удалить всё, что не используется (неиспользуемые образы, остановленные контейнеры, тома, которые не использует ни один контейнер, билд-кеш)

```sudo docker system prune```

_Документация: Примеры обращений к эндпоинтам находятся по адресу:_

*http://localhost:8000/redoc/*

_Разработчики: Абакумов Максим, Андреев Ярослав, Быков Евгений._
