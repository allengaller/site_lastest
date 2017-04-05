---
title: install django on mac
update: 2017-01-12 17:43:23
categories:
- python
tags: 
- install
- python
- django
- docker
---

# install on mac (bare metal)

# install on mac (docker)
- download & install docker for mac;
    - link: https://docs.docker.com/compose/django/;
    - Define the project components;
        1. create folder;
        2. create Dockerfile;
            ```
             FROM python:2.7
             ENV PYTHONUNBUFFERED 1
             RUN mkdir /code
             WORKDIR /code
             ADD requirements.txt /code/
             RUN pip install -r requirements.txt
             ADD . /code/
            ```
        3. create requirements.txt;
            ```
             Django
             psycopg2
            ```
        4. create docker-compose.yml
            ```
             version: '2'
             services:
               db:
                 image: postgres
               web:
                 build: .
                 command: python manage.py runserver 0.0.0.0:8000
                 volumes:
                   - .:/code
                 ports:
                   - "8000:8000"
                 depends_on:
                   - db
            ```
    - Create a Django project;
        1. goto root dir;
        2. docker-compose run web django-admin.py startproject composeexample .
        3. ls -l; sudo chown -R $USER:$USER .;
    - Connect the database;
        1. edit composeexample/settings.py;
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
        2. $ docker-compose up