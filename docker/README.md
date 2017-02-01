## [Docker](https://www.docker.com)

Docker позволяет удобно и быстро разворачивать проекты для разработки и продакшена

Для его работы необходимо установить соответствующее ПО на свой компьютер (см. на сайте)

Прежде всего необходимо определиться с набором контейнеров:

- Nginx
- Python
- Postgres

Для простой упаковки контейнеров применяется расширение [docker-compose](https://docs.docker.com/compose/overview/)

Каждый контейнер должен быть описан в файле `docker-compose.yml`

-------------

### Пример 1 (см в папке example1)

- создаем файл app.py, представляющий собой простейший питоновский сервер
- создаем файл requirements.txt, в котором описываем необходимые модули для питона
- создаем файл Dockerfile, в котором описываем настройки кондейнера для контейнера с питоном

можно обойтись и без этого файла, но тогда все команды придется прописать одной строкой в будущем. То есть этот файл используется для глубокой кастомизации каждого контейнера

- создаем файл docker-compose.yml, в котором описываем как собирать все контейнеры

Теперь нам доступны команды для docker-compose

`docker-compose up` - билдит и запускает контейнеры, описаные в docker-compose.yml

Теперь по ссылке `http://localhost:5000` или `http://0.0.0.0:5000` отвечает наше приложение

Если мы отредактируем исходники в папке разработки, то изменится и код в docker-контейнере

`docker-compose up -d` - билдит и запускает контейнеры, описаные в docker-compose.yml в режиме daemon
`docker-compose run web something` - выполняет команду внутри работающего docker контейнера
`docker-compose stop` - останавливает контейнеры
`docker-compose down --volumes` - удаляет контейнеры и все созданные ими файлы

-----------------------

### Пример 2 (см в папке example2)

По мотивам [Quickstart: Compose and Django](https://docs.docker.com/compose/django/)

- создаем файл Dockerfile, в котором описываем настройки кондейнера для контейнера с питоном
- создаем файл requirements.txt, в котором описываем необходимые модули для питона
- создаем файл docker-compose.yml, в котором описываем как собирать все контейнеры

`docker-compose run web django-admin.py startproject composeexample .` - билдим и запускаем внутри контейнера web команду на создание проекта

В итоге получаем структуру django-проекта в директории

- редактируем composeexample/settings.py

```
DATABASES = {
     'default': {
         'ENGINE': 'django.db.backends.postgresql',
         'NAME': 'postgres',
         'USER': 'postgres',
         'HOST': 'db',
         'PORT': 5432,
     }
 }
```

- `docker-compose up -d` - и нужные контейнеры запущены

- иногда возникают конфликты между контейнерами на компе, тогда делаем:

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

В итоге получили готовый Django сервер

------------------

### Пример 3 (см в папке example2)

По мотивам [How to deploy Django with Docker](https://www.stavros.io/posts/how-deploy-django-docker/)

- docker-compose is good in dev, but not useful in production
- for easy dev write docker-compose.yml
- clone django-project to path

`docker-compose run web /code/manage.py whatever` - любые команды внутри контейнера

Имеем работающую связку из Django, Postgres и сервера

#### Production

- we’ll start with the Dockerfile, the Dockerfile is completely separate from docker-compose.yml

**Продолжить!**

------------------

### Ссылки

1. [Compose for production](https://docs.docker.com/compose/production/)
2. [Production-ready Django on Docker](https://github.com/morninj/django-docker)
3. [This is a production-ready setup for running Django on Docker.](https://github.com/morninj/django-docker)


Using Gitlab to build your images: `docker push registry.gitlab.com/vitaliyharchenko/harchenkopro`

Then push on production

```
docker pull registry.gitlab.com/vitaliyharchenko/harchenkopro
docker run --net=host --env-file=/your/env.file --name=project registry.gitlab.com/vitaliyharchenko/harchenkopro
```

1.3. [Используем Gitlab-CI для автоматического выкладывания всякого](https://habrahabr.ru/post/316054/)
