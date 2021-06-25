# docker-manticore HappyDay by Regul
- докер-контейнер мантикоры 
- тестировался только на MacOS 11.2.3 и на Ubuntu 18.04.3 LTS (Bionic Beaver)
- под windows работать не будет 

## сборка
1. `cp .env.example .env`
2. в папке проекта делаем `sudo php artisan manticore:config:create config/manticore/manticore.conf.example {PATH_TO_DOCKER_DIR}etc/sphinx.conf --index_prefix=/var/lib/manticore/data --log_path=/var/log/manticore/searchd.log --query_log_path=/var/log/manticore/query.log --pid_path=/var/run/manticore/searchd.pid --binlog_path=`
3. возможно настраиваем внешние порты в `.env`
4. `sudo docker-compose up -d`
5. `sudo docker-compose exec manticore gosu manticore indexer --all --rotate`
6. `sudo docker-compose restart manticore`
7. `sudo mysql -P{MANTICORE_MYSQL_PORT} -h0 -e 'select * from searchIndex\G'`
8. меняем права `sudo chmod 0770 bash-reindex`
9. добавляем в кронтаб рута реиндекс: 
```
sudo crontab -e
* * * * * {PATH_TO_DOCKER_DIR}/bash-reindex
```

## запуск
`sudo docker-compose up -d`

## остановка
`sudo docker-compose stop`

## перезапуск
`sudo docker-compose restart manticore`

## внутрь контейнера
`sudo docker-compose exec manticore bash`

## внутрь контейнера под рутом
`sudo docker-compose exec -u 0 manticore bash`

## ротация индексов
### главный индекс
`sudo docker-compose exec manticore gosu manticore indexer --all --rotate`

## глянуть логи
`sudo docker-compose logs manticore` или в папке `logs`

## ребилд контейнера
даже, если конфиг и имидж не изменились
`docker-compose up -d --no-deps --build --force-recreate manticore`

## если есть mysql-client
`sudo mysql -P{MANTICORE_MYSQL_PORT} -h0 -e 'select * from searchIndex\G'`