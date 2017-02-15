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

### Пример 3 (см в папке example3)

По мотивам [How to deploy Django with Docker](https://www.stavros.io/posts/how-deploy-django-docker/)

- docker-compose is good in dev, but not useful in production
- for easy dev write docker-compose.yml
- clone django-project to path

`docker-compose run web /code/manage.py whatever` - любые команды внутри контейнера

Имеем работающую связку из Django, Postgres и сервера

#### Production

- we’ll start with the Dockerfile, the Dockerfile is completely separate from docker-compose.yml

-----------------

### Пример 4 (папка example4)

по мотивам: [Build and Deploy a Python Web App on Docker](https://www.distelli.com/docs/tutorials/build-and-deploy-python-with-docker)

-----------------

### Пример 5

По мотивам: [Django Development With Docker Compose and Machine](https://realpython.com/blog/python/django-development-with-docker-compose-and-machine/)

`docker-machine create -d virtualbox dev;` создаем и запускаем виртуальную машину

`eval $(docker-machine env dev)` just point Docker at the dev machine

`docker-machine ls` view the currently running Machines

`docker-compose build`  `docker-compose up -d` get the containers running, build the images and then start the services

`docker-compose run web /usr/local/bin/python manage.py migrate` run commands in container web

`docker-machine ip dev` Grab the IP associated with Docker Machine

`docker-compose ps` see different containers

`docker-compose run web env` see env variables in web container

--------------------








### Ссылки

1. [Compose for production](https://docs.docker.com/compose/production/)
2. [Production-ready Django on Docker](https://github.com/morninj/django-docker)
4. [Dockerized Django+React starter project](https://github.com/elielagmay/docker-django-react-seed)
5. [Just a Django Docker image build example](https://github.com/davidkwast/docker-django-example)
6. [Автоматическое развёртывание Django из GitLab](https://habrahabr.ru/post/316054/)
7. [Django, uWSGI and Nginx in a container, using Supervisord](https://github.com/dockerfiles/django-uwsgi-nginx)
8. [Hello world in Django with Docker](https://github.com/hoh/hello-django-docker)
9. [Dockerfile for quickly running Django projects in a Docker container.](https://github.com/praekeltfoundation/docker-django-bootstrap)
11. [Поднимаем сложный проект на Django с использованием Docker](https://habrahabr.ru/post/272811/)