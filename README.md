[![Django-app workflow](https://github.com/Andrei191/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/Andrei191/yamdb_final/actions/workflows/yamdb_workflow.yml)

# REST API для сервиса YaMDb — базы отзывов

Проект YaMDb собирает отзывы пользователей на произведения. Произведения делятся на категории (книги, фильмы и тд)
Произведению может быть присвоен жанр из списка предустановленных (сказка, драма, артхаус и тд)
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
Пользователи оставляют к произведениям текстовые отзывы и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти)
Из множества оценок автоматически высчитывается средняя оценка произведения. К каждому отзыву можно оставить комментарий.

---

## Workflow: 

* Тестирование проекта (pytest, flake8).
* Сборка и публикация образа на DockerHub.
* Автоматический деплой на удаленный сервер.
* Отправка уведомления в телеграм-чат.

## Как запустить проект

### Клонировать репозиторий и перейти в него в командной строке

```
git clone git@github.com:Andrei191/yamdb_final.git
```

#### Выполните вход на свой удаленный сервер

#### Установите docker на сервер

```
sudo apt install docker.io 
```

#### Установите docker-compose на сервер(актуальная версия [тут](https://github.com/docker/compose/releases))

```
sudo curl -L "https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

#### Затем необходимо задать правильные разрешения, чтобы сделать команду docker-compose исполняемой

```
sudo chmod +x /usr/local/bin/docker-compose
```

#### Скопируйте файлы docker-compose.yaml и nginx/default.conf из проекта на сервер

```
scp docker-compose.yaml <username>@<host>:/home/<username>/docker-compose.yaml
```

```
scp -r nginx <username>@<host>:/home/<username>/
```

#### Добавьте в Secrets GitHub переменные окружения для работы

```
DB_ENGINE=django.db.backends.postgresql
DB_HOST=db
DB_NAME=postgres
DB_PASSWORD=postgres
DB_PORT=5432
DB_USER=postgres

DOCKER_PASSWORD=<Docker password>
DOCKER_USERNAME=<Docker username>

USER=<username для подключения к серверу>
HOST=<IP сервера>
PASSPHRASE=<пароль для сервера, если он установлен>
SSH_KEY=<ваш SSH ключ(cat ~/.ssh/id_rsa)>

TG_CHAT_ID=<ID чата, в который придет сообщение>
TELEGRAM_TOKEN=<токен вашего бота>
```

#### Запушить на Github. После успешного деплоя зайдите на боевой сервер и выполните команды

#### Собрать статические файлы в STATIC_ROOT

```
docker-compose exec web python3 manage.py collectstatic --noinput
```

#### Применить миграции

```
docker-compose exec web python3 manage.py migrate --noinput
```

#### Заполнить базу данных

```
docker-compose exec web python3 manage.py loaddata fixtures.json
```

#### Создать суперпользователя Django

```
docker-compose exec web python manage.py createsuperuser
```

#### Проект будет доступен по адресу

```
http://{ваш ip}/api/v1/
```

#### Документация API

```
http://{ваш IP}/redoc/
```

#### [Образ на DockerHub](https://hub.docker.com/repository/docker/andrei191000/api_yamdb)

---

#### Данный проект доступен по [ссылке](http://178.154.198.167/api/v1/)

Авторы проекта:
Якушкин Вячеслав,
Павел Середа,
<a href="https://github.com/Andrei191"> Карасев Андрей </a>.
