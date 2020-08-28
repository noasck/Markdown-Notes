# Dockerize simple Python app
### without WSGI

``` bash
root
├── docker-compose.yml
├── .env
└── services
    └── web
        ├── Dockerfile
        ├── project
        │   └── __init__.py
        ├── requirements.txt
        ├── start.py
        └── tests
            ├── conftest.py
            └── fixtures.py
```
Dockerfile
``` bash
FROM python:3.7.8-slim-buster

WORKDIR /usr/src/app

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN apt-get update && apt-get install -y netcat

RUN pip install --upgrade pip
COPY requirements.txt /usr/src/app/requirements.txt
RUN pip install -r requirements.txt

COPY . /usr/src/app/
```

Docker-compose.yml

``` yaml
version: '3.7'
services:
  web:
    build: ./services/web
    command: python start.py
    volumes:
      - ./services/web/:/usr/src/app/
    ports:
      - 1234:1234
    env_file:
      - ./.env
  db:
    image: postgres:12-alpine
    ports:
    - 5432:5432
    volumes:
      - postgres_data:/var/lib/postgresql/data/
    environment:
      - POSTGRES_USER=user1234
      - POSTGRES_PASSWORD=pass1234
      - POSTGRES_DB=sampledb

volumes:
  postgres_data:
```

conftest.py

``` python
import pytest

pytest_plugins = ['fixtures']
```
