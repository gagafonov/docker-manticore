# docker-manticore
докер-контейнер мантикоры

## сборка
1. `cp .env.example .env`
2. в папке проекта делаем `sudo php artisan manticore:config:create config/manticore/manticore.conf.example {PATH_TO_DOCKER_DIR}etc/sphinx.conf --index_prefix=/var/lib/manticore/data --log_path=/var/log/manticore/searchd.log --query_log_path=/var/log/manticore/query.log --pid_path=/var/run/manticore/searchd.pid --binlog_path=`
3. возможно, берем настройки `searchd` из `etc/sphinx.conf.example` и настраиваем внешние порты в `.env`
4. `sudo docker-compose up -d`
5. `sudo docker-compose exec manticore gosu manticore indexer --all --rotate`
6. `sudo docker-compose restart manticore`
7. `sudo mysql -P{MANTICORE_MYSQL_PORT} -h0 -e 'select * from searchIndex\G'`

## запуск
`sudo docker-compose up -d`

## остановка
`sudo docker-compose stop`

## перезапуск
`sudo docker-compose restart manticore`

## внутрь контейнера
`sudo docker-compose exec manticore bash`

## ротация индексов
### главный индекс
`sudo docker-compose exec manticore gosu manticore indexer --all --rotate`
### дельта и глобалы – это не работает
`sudo docker-compose exec manticore gosu manticore indexer listingsDeltaIndex globalUidIndex dealsDeltaIndex globalDealUidIndex leadsDeltaIndex globalLeadUidIndex --rotate`

## глянуть логи
`sudo docker-compose logs manticore`

## ребилд контейнера
даже, если конфиг и имидж не изменились
`docker-compose up -d --no-deps --build --force-recreate manticore`

## если есть mysql-client
`sudo mysql -P{MANTICORE_MYSQL_PORT} -h0 -e 'select * from searchIndex\G'`