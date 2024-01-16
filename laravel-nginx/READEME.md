# laravel-nginx

## Introduction

Build a simple laravel development environment with docker-compose.

## Usage
### setup
```bash
$ make create-project # Install the latest Laravel project
$ make install-recommend-packages # Not required
docker compose exec app composer require laravel/breeze --dev
docker compose exec app php artisan breeze:install vue
$ make front-install
$ make create-front
```

### .gitignoreの編集
- src/.gitignoreに下記を追加する
  ```
  /public/css
  /public/js
  /public/mix-manifest.json
  /.yarn
  !.yarn/releases/*
  ```

### viteまわりでエラーが出たら
1. `package.json`に`"type": "module"`を追加
2. `tailwindcss/postcss configs`の拡張子を`cjs`に変更
3. `tailwindcss/postcss configs`ファイル内の`export default {`を`module.exports = {`に変更
4. scriptsの追加
    ```
    "scripts": {
        "dev": "vite  --no-open --host",
        "build": "vite build",
    }
    ```

## If you edit Docker-related files after build
```
make refreash-rebuild
```
- src/.gitignoreに下記を追加する
  ```
  /public/css
  /public/js
  /public/mix-manifest.json
  /.yarn
  !.yarn/releases/*
  ```

## URL
- http://localhost


## Container structure

```bash
├── app
├── web
└── db
```

### app container

- Base image
  - [php](https://hub.docker.com/_/php):8.0-fpm-buster
  - [composer](https://hub.docker.com/_/composer):2.7
  - [node](https://hub.docker.com/_/node):lts-bullseye-slim

### web container

- Base image
  - [nginx](https://hub.docker.com/_/nginx):1.20-alpine
  

### db container

- Base image
  - [mysql](https://hub.docker.com/_/mysql):8.0

#### Persistent MySQL Storage

By default, the [named volume](https://docs.docker.com/compose/compose-file/#volumes) is mounted, so MySQL data remains even if the container is destroyed.
If you want to delete MySQL data intentionally, execute the following command.

```bash
$ docker-compose down -v && docker-compose up
```

### policy
- tfstateをAWSのストレージサービスであるS3に保存