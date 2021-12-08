## WordPressのテスト環境
### 実行方法
```
docker-compose up -d
```
※初回は2回実行しないとDBが立ち上がらない。

### コンテナに入る方法
execする。
```
docker exec -it wordpress /bin/bash
```

## プラグインのスキャフォールド
あらかじめSVNをインストールしておく。
また、`localhost:8080`より、インストールまで済ませる。
```
mkdir /var/www/html/wp-content/plugins/test-plugin
cd /var/www/html
wp --allow-root scaffold plugin test-plugin
```

## テスト用のワードプレスをインストール
あらかじめ`mariadb`のrootのユーザのログイン方式を変更する
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
cd /var/www/html/wp-content/plugins/test-plugin
bash bin/install-wp-tests.sh wordpress root password localhost:/var/run/mysqld/mysqld.sock
```

## 参考資料
- [WordPressの開発環境をDockerで構築する[その1]](https://samurai-project.com/articles/3397)
- [WordPressの開発環境をDockerで構築する[その2]](https://samurai-project.com/articles/3423)
- [WordPressの開発環境をDockerで構築する[その3]](https://samurai-project.com/articles/3422)
- [Dockerで作ったWordPress環境にWP-CLIを追加する方法](https://samurai-project.com/articles/3413)
- [Docker を使って WordPress plugin にテストのためのファイルを追加しよう](https://futureys.tokyo/lets-add-files-for-test-into-wordpress-plugin-by-docker/)
- [composer Quick reference](https://hub.docker.com/_/composer)
- [dockerからmysqlに接続できない](https://qiita.com/KOBA-RYOTA/items/3cf5070b54845e151034)

## 手順にないけどやること
- SVNインストール
    - `apt-get update`
    - `apt-get install subversion`
- vimインストール
- mariadbインストール
    - `apt-get update`
    - `apt install mariadb-common`
    - `apt install mariadb-client`
    - `apt install mariadb-server`
    - `service mariadb restart`
