# laravel + Docker　開発環境

## 概要

dockerを使用してlaravel,Nginx,MySQL,Swagger UIを連携したアプリケーション開発環境。
wsl2とubuntuを使用。

## ディレクトリ構成

```
.
├── container/      # Dockerコンテナの設定ファイル群
│   ├── mysql/
│   ├── nginx/
│   └── php/
├── doc/            # ドキュメント関連ファイル
│   └── api/
│       └── openapi.yaml
├── src/            # Laravelアプリケーションのソースコード
├── .gitignore
├── docker-compose.yml
└── README.md
```

## 環境構築手順
1.  **リポジトリをクローン**
    ```bash
    git clone [https://github.com/yahata00009/laravel-project.git](https://github.com/yahata00009/laravel-project.git)
    cd laravel-project
    ```

2.  **.envファイルの作成と編集**
    Laravelの設定ファイル`.env`を作成します。
    ```bash
    cp ./src/.env.example ./src/.env
    ```
    作成した`./src/.env`ファイルを開き、データベースのホスト名をDockerのサービス名に変更します。
    ```ini
    # 変更前
    DB_HOST=127.0.0.1
    # 変更後
    DB_HOST=mysql
    ```

3.  **コンテナのビルドと起動**
    ```bash
    docker compose up -d --build
    ```

4.  **PHPパッケージのインストール**
    `php`コンテナ内で`composer install`を実行します。
    ```bash
    docker compose exec php composer install
    ```

5.  **アプリケーションキーの生成**
    Laravelの暗号化に必要なキーを生成します。
    ```bash
    docker compose exec php php artisan key:generate
    ```

6.  **データベースマイグレーション**
    データベースにテーブルを作成します。
    ```bash
    docker compose exec php php artisan migrate
    ```

7.  **書き込み権限の設定**
    Laravelが`storage`ディレクトリ等に書き込みを行えるように、パーミッションを設定します。
    ```bash
    sudo chmod -R 777 ./src/storage
    sudo chmod -R 777 ./src/bootstrap/cache
    ```
## アクセスURL一覧
* **Laravelアプリケーション:**
    * [http://localhost:8080](http://localhost:8080)
    * Laravelのウェルカムページが表示されます。
* **Swagger UI (APIドキュメント):**
    * [http://localhost:8081](http://localhost:8081)
    * API仕様書の画面が表示されます。
