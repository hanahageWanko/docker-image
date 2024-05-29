# docker + laravel + terraform + AWS 🐳

## Introduction

Build a simple laravel development environment with docker-compose.

## Prerequisites

-   Docker Desktop

## Setup

### 環境構築

```bash
$ make create-project # Install the latest Laravel project
$ make install-recommend-packages # Not required
$ make front-install
sed -i -e $'1s/^/.yarn\\/\\\n\\!.yarn\\/release\\/*\\\n/' src/.gitignore
$ make create-front
docker compose exec app composer require laravel/breeze --dev
docker compose exec app php artisan breeze:install vue
docker compose exec app php artisan key:generate
```

## コマンドの追加

1. src/package.json への scripts の追加
    ```
    "scripts": {
        "dev": "vite  --no-open --host",
        "build": "vite build",
    }
    ```
2. lodash 使用箇所の削除 or コメントアウト

```js
# src/resources/js/bootstrap.js
## 下記に行をコメントアウト or 削除
// import _ from 'lodash';
// window._ = _;
```

## 対策

### .gitignore の編集

-   src/.gitignore に下記を追加する(追加されていれば OK)
    ```
    /public/css
    /public/js
    /public/mix-manifest.json
    /.yarn
    !.yarn/releases/*
    ```

### vite まわりでエラーが出たら

1. `package.json`に`"type": "module"`を追加
2. `tailwindcss.configs/postcss.configs`の拡張子を`cjs`に変更
3. `tailwindcss.configs/postcss.configs`ファイル内の`export default {`を`module.exports = {`に変更

## If you edit Docker-related files after build

```
make refreash-rebuild
```

## URL

-   http://localhost

## Container structure

```bash
├── app
├── web
└── db
```

### app container

-   Base image
    -   [php](https://hub.docker.com/_/php):8.0-fpm-buster
    -   [composer](https://hub.docker.com/_/composer):2.7
    -   [node](https://hub.docker.com/_/node):lts-bullseye-slim

### web container

-   Base image
    -   [nginx](https://hub.docker.com/_/nginx):1.20-alpine

### db container

-   Base image
    -   [mysql](https://hub.docker.com/_/mysql):8.0

#### Persistent MySQL Storage

By default, the [named volume](https://docs.docker.com/compose/compose-file/#volumes) is mounted, so MySQL data remains even if the container is destroyed.
If you want to delete MySQL data intentionally, execute the following command.

```bash
$ docker-compose down -v && docker-compose up
```
