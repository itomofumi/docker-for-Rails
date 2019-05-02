# Doker-for-Rails
## 概要
windows10 HOMEでのrails環境構築

サクッと作る用

## 手順
### 0. 下準備
Windows10 Homeの場合
DockerToolBoxをダウンロードしてインストール

`https://www.docker.com/get-docker`

### 1. ファイル生成
- docker-compose.yml
- Dockerfile
- Gemfile
- Gemfile.loc

をそれぞれ作成する

### 2. アプリを生成
rails newでアプリを生成

`docker-compose run web rails new . --force --database=mysql`

### 3. build
`docker-compose build`

### 4. config/database.yml を補正
アプリが生成されたら、config/database.ymlの`password` と `host`をdocker-compose.ymlの設定と合わせる

```
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: root
  password: password  # docker-compose.yml　MYSQL_ROOT_PASSWORDの設定に補正
  host: db  # docker-compose.yml services:depends_on 設定に補正

development:
  <<: *default
  database: app_development
```

### 5. コンテナＵＰ
`docker-compose up -d`

### 6. MySQLのインスタンスを作成
Rails上にMySQLのインスタンスを作成

`docker-compose run web bundle exec rake db:create`

### 7. IPを確認してブラウザにアクセス
下記コマンドでdockerマシンのＩＰを確認

`docker-machine ip`

ブラウザを起動して確認したＩＰでアクセス

`http://192.168.99.100:3000`

アクセスして`Yay! You’re on Rails!`が表示できれば完了です