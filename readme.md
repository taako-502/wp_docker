## WordPressのテスト環境
実行方法
```
docker-compose up -d
```

wp-cli
コンテナIDを確認する。
```
docker ps
```

コンテナIDに対してexecする。
```
docker exec -it {コンテナID} bash
```


## 参考資料
- [WordPressの開発環境をDockerで構築する[その1]](https://samurai-project.com/articles/3397)
- [WordPressの開発環境をDockerで構築する[その2]](https://samurai-project.com/articles/3423)
- [WordPressの開発環境をDockerで構築する[その3]](https://samurai-project.com/articles/3422)
- [Dockerで作ったWordPress環境にWP-CLIを追加する方法](https://samurai-project.com/articles/3413)
