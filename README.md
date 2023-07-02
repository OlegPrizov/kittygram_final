![Main Kittygram Workflow](https://github.com/OlegPrizov/kittygram_final/actions/workflows/main.yml/badge.svg)

Проект Kittygram – форум для публикации котов, где можно выбрать их цвет, год рождения, загрузить фото и присвоить им какие-либо достижения. 
kittygram_domain: https://naprimerrrkittygram.ddns.net/

Проект Taski – приложение для постановки задач. 
taski_domain: https://taskinaprimerrr.ddns.net/
 
## Технологии 
1. Python 3.10.6 
2. Django==3.2.3 
3. djangorestframework==3.12.4 
4. djoser==2.1.0 
5. gunicorn 20.1.0 
6. Docker

## Как развернуть проект на сервере

1. Устанавливаем Docker Compose на сервер: поочерёдно выполните следующие команды
```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```

2. Запускаем Docker Compose на сервере
Cоздайте на сервере пустой файл docker-compose.production.yml в нужной директории и с помощью редактора nano добавьте в него содержимое из docker-compose.production.yml (https://github.com/OlegPrizov/kittygram_final/blob/main/docker-compose.production.yml)

Создайте и заполните .env файл в той же директории

Выполните следующую команду из директории с файлом docker-compose.production.yml
```
docker-compose.production.yml
```

Выполните миграции, соберите статические файлы бэкенда и скопируйте их в /backend_static/static/
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

На сервере в редакторе nano откройте конфиг Nginx: nano /etc/nginx/sites-enabled/default. Измените настройки location в секции server на следуюшие:
```
# Всё до этой строки оставляем как было.
    location / {
        proxy_pass http://127.0.0.1:9000;
    }
# Ниже ничего менять не нужно.
```

Перезагрузите конфиг Nginx
```
sudo service nginx reload
```

## Шаблон наполнения env-файла 

``` 
POSTGRES_USER=
POSTGRES_PASSWORD=
POSTGRES_DB=
DB_HOST=
DB_PORT=
SECRET_KEY=
``` 

## Автор 

[Олег Призов](https://github.com/OlegPrizov) 

repo_owner: OlegPrizov
kittygram_domain: https://naprimerrrkittygram.ddns.net/
taski_domain: https://taskinaprimerrr.ddns.net/
dockerhub_username: olegprizov
