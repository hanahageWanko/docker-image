# laravel-nginx

## setup
```
make create-project

docker compose exec app composer require laravel/breeze --dev
docker compose exec app php artisan breeze:install vue
docker compose exec app yarn

make front-install
make create-front
docker compose exec app yarn add -D vite
docker compose exec app yarn add -D laravel-vite-plugin

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

### docker-compose.ymlへの追加
```
# app
  ports:
    - target: 5173
      published: ${WEB_PORT:-5173}
      protocol: tcp
      mode: host
```

## Dockerfileやdokcer-compose.ymlを変更した時
- `make freash-rebuild`を実行
- src/.gitignoreに下記を追加する
  ```
  /public/css
  /public/js
  /public/mix-manifest.json
  /.yarn
  !.yarn/releases/*
  ```