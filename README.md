# docker-manticore
докер-контейнер мантикоры

## сборка
1. `cp .env.example .env`
2. в папке проекта делаем `sudo php artisan manticore:config:create config/manticore/manticore.conf.example {PATH_TO_DOCKER_DIR}/sphinx.conf --index_prefix=/var/lib/manticore/data --log_path=/var/log/manticore/searchd.log --query_log_path=/var/log/manticore/query.log --pid_path=/var/run/manticore/searchd.pid --binlog_path=`
3. возможно, берем настройки `searchd` из `etc/sphinx.conf.example` и настраиваем внешние порты в `docker-compose.yml`
4. `sudo docker-compose up -d`
5. `sudo docker-compose exec manticore hostname -i` - копируем ip-контейнера
6. вносим правка etc/manticore.conf: заменяем `listen = 0.0.0.0:9312` на `listen = ip-контейнера:9312`
7. `sudo docker-compose restart manticore`
8. `sudo docker-compose exec manticore gosu manticore indexer --all --rotate`
9. `sudo docker-compose restart manticore`
10. `sudo mysql -P9306 -h0 -e 'select * from searchIndex\G'`

## запуск
1. `docker-compose up -d`

## остановка
`docker-compose stop`

## перезапуск
`docker-compose restart manticore`

## внутрь контейнера
`docker-compose exec manticore bash`

## ротация индексов
### главный индекс
`docker-compose exec manticore gosu manticore indexer --all --rotate`
### дельта и глобалы – это не работает
`docker-compose exec manticore gosu manticore indexer listingsDeltaIndex globalUidIndex dealsDeltaIndex globalDealUidIndex leadsDeltaIndex globalLeadUidIndex --rotate`

## глянуть логи
`docker-compose logs manticore`

## ребилд контейнера
даже, если конфиг и имидж не изменились
`docker-compose up -d --no-deps --build --force-recreate manticore`

## если есть mysql-client
`mysql -P9306 -h0 -e 'select * from searchIndex\G'`