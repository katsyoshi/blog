+++

title = "野良Gitサーバにuploadできなかった話"
date = 2014-06-22
comments = true
categories = "git tech diary"
+++

今日もろくでもない理由で嵌った。

## 嵌った理由
野良Gitサーバを立てているのでそこに新しいリポジトリを作成した。

で、リモートを追加し、ブランチを追加しようとすると

```
$ git remote add master ssh://myhost/my_project.git
$ git push origin master -f
error: src refspec master does not match any.
error: failed to push some refs to 'ssh://myhost/my_project.git'
```

というようなエラーが出てきてアップロードできなかった。
そのため理由を探ってたら[同じ理由](http://d.hatena.ne.jp/nishiohirokazu/20110304/1299229916)があった。

そこで `git log` としたら何もでてこなかったので

```
$ git add .
$ git commit -m 'first commit'
$ git push -u origin master
```

リモートリポジトリに追加できました。
ちゃんちゃん
