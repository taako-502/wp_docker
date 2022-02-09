# WordPressのテスト環境
## 概要
WordPressテーマの自動テスト環境の手順を以下に記述する。

## もくじ
ざっくり以下の手順でテスト環境を構築する。
`hepere`は私が開発しているテーマ名です。<br>
実際にはテストするテーマ名に置き換えてください。<br>

1. DockerでWordPressのコンテナを作成する。
2. テーマのスキャフォールド
3. コンテナ内にテスト用の資材を準備する。
    - mariadb
    - phpunit
4. テストを実行する。

## DockerでWordPressのコンテナを作成する。
```
docker-compose up -d
```
※初回は2回実行しないとDBが立ち上がらない。

### コンテナに入る方法
execする。
```
docker exec -it wordpress /bin/bash
```

## テーマのスキャフォールド
`SVN`をインストール。
```
apt-get update
apt-get install subversion
```

また、ブラウザから`localhost:8080`を開き、インストールまで済ませる。<br>
または、`wp-cli`のコマンドでインストールすることも可能。<br>
```
wp --allow-root core install --url=http://localhost:8080/ --title=test --admin_user=admin --admin_password=password --admin_email=admin@example.com
```

次に、スキャフォールドを実行する。

```
cd /var/www/html
wp --allow-root scaffold theme-tests hepere
```

これで、phpunitに必要なファイルが作成される。

## コンテナ内にテスト用の資材を準備
まずは`mariadb`をインストールする。<br>
※上から順番に実行しないと、最初からやり直しになる可能性あり。
```
apt-get update
apt install mariadb-common
apt install mariadb-client
apt install mariadb-server
service mariadb restart
```

### テスト用のワードプレスをインストール
テスト用のワードプレスをインストールする。
```
cd /var/www/html/wp-content/themes/hepere
bash bin/install-wp-tests.sh wordpress root password localhost:/var/run/mysqld/mysqld.sock
```

## phpunitインストール
`phpunit`をインストールする。

あらかじめテーマがあるフォルダに移動する。
```
cd /var/www/html/wp-content/themes/hepere
```

まずは、`composer.json`を作成する。
`composer init`使用して、`composer.json` を作成する方法もあるが、本手順書では、直接作成する。<br>
なお、`composer init`で作成する場合は、`require-dev`に`phpunit/phpunit:5.7.27`と`yoast/phpunit-polyfills`を追加する必要がある。

では、ファイルを編集するために、`vim`をインストールする。
```
apt-get update
apt-get install vim
```

次に、`composer.json`を作成し、中身を以下のようにする。
```
{
    "name": "wp_theme/hepere",
    "type": "composer-theme",
    "license": "GPL-3.0-or-later",
    "minimum-stability": "stable",
    "scripts": {
      "test": "phpunit"
    },
    "require-dev": {
        "phpunit/phpunit": "5.7.21",
        "yoast/phpunit-polyfills": "^1.0"
    }
}
```

以下のコマンドを実行する。
```
composer update
composer install
```

## テストを実行
以下のコマンドでテストを実行する。
```
composer test
```

もしデータベースに接続できない場合は、以下のコマンドを使用して`mariadb`を再起動すること。
```
service mariadb restart
```

## 参考資料
- [WordPress テスト入門 – 2章 –](https://kunoichiwp.com/faq/2434)
- [wp scaffold theme-tests](https://developer.wordpress.org/cli/commands/scaffold/theme-tests/)
- [Dockerで作ったWordPress環境にWP-CLIを追加する方法](https://samurai-project.com/articles/3413)
- [Docker を使って WordPress plugin にテストのためのファイルを追加しよう](https://futureys.tokyo/lets-add-files-for-test-into-wordpress-plugin-by-docker/)
- [composer Quick reference](https://hub.docker.com/_/composer)
- [dockerからmysqlに接続できない](https://qiita.com/KOBA-RYOTA/items/3cf5070b54845e151034)
- [PHP: PHPUnit](https://make.wordpress.org/core/handbook/testing/automated-testing/phpunit/)
- [WordPressのPHPUnitが刷新、プラグイン・テーマはポリフィル利用を](https://capitalp.jp/2021/10/14/wordpres-phpunit-updated/)
