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

### wp-cli使用方法
`docker-compose run --rm `の後にコマンドを入力する。
以下は例。
```
docker-compose run --rm wpcli --info
```

## プラグインのスキャフォールド
```
docker exec -it wordpress /bin/bash
wp --allow-root scaffold plugin-tests test-plugin
```

## 参考資料
- [WordPressの開発環境をDockerで構築する[その1]](https://samurai-project.com/articles/3397)
- [WordPressの開発環境をDockerで構築する[その2]](https://samurai-project.com/articles/3423)
- [WordPressの開発環境をDockerで構築する[その3]](https://samurai-project.com/articles/3422)
- [Dockerで作ったWordPress環境にWP-CLIを追加する方法](https://samurai-project.com/articles/3413)
- [Docker を使って WordPress plugin にテストのためのファイルを追加しよう](https://futureys.tokyo/lets-add-files-for-test-into-wordpress-plugin-by-docker/)
- [composer Quick reference](https://hub.docker.com/_/composer)

## 手順にないけどやること
- SVNインストール
- vimインストール
- mariadbインストール
    - `apt-get update`
    - `apt install mariadb-client`
