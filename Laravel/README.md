# storage-system

## 前提
- Git
- Docker for Mac
  - V3.4.0以上
- Docker Compose
  - v2以上

```
$ git --version
git version 2.31.1
$ docker --version
Docker version 20.10.10, build b485636
$ docker compose version    
Docker Compose version v2.1.1
```

## Get Started
```
make create-project
```

## Create an existing Laravel project
```
make install
```

## コンテナ構成
```
src
 ├── app
 ├── web
 └── db
```

### app コンテナ
- アプリケーションサーバーのコンテナ
- PHPのバージョンは8.0系を利用する
- Laravelのサーバ要件を満たす
  - PHP7.3以上
  - BCMath PHP 拡張
  - Ctype PHP 拡張
  - Fileinfo PHP 拡張
  - JSON PHP 拡張
  - Mbstring PHP 拡張
  - OpenSSL PHP 拡張
  - PDO PHP 拡張
  - Tokenizer PHP 拡張
  - XML PHP 拡張
- php, composer のベースイメージを利用

### web コンテナ
- ウェブサーバーのコンテナ
- HTTPリクエストを受けて、HTTPレスポンスを返す
- phpファイルへのアクセスはappコンテナに投げる
- nginx のベースイメージを利用

### db コンテナ
- データベースサーバーのコンテナ
- MySQLのバージョンは8.0系を利用
- mysql/mysql-server のベースイメージを利用
  - Docker Hub公式イメージではなく、OracleのMySQLチームがメンテしているDockerイメージを利用します

## Base image
### app container
- php:8.3-fpm-bullseye
- composer:2.6

### web container
- nginx:1.25

### db container
- mysql/mysql-server:8.0

### mailpit container
- axllent/mailpit

## その他
```
# バックエンド側のパッケージ導入
docker compose exec api composer update
# or 
docker compose exec api composer install

# Inertia利用時は実施
docker compose exec api php artisan jetstream:install inertia

# migrateの実施
docker compose exec api php artisan migrate

# docker compose exec api composer install
```