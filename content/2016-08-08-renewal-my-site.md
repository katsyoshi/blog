+++
path = "/blog/2016/08/08/renewal-my-site"
layout = "post"
title = "RENEWAL MY SITE!!!"
date = 2016-08-08
comments = true
categories = "diary h2 h2o ssl"
+++

ってのは嘘ではないですが、嘘に近いです。
実はおとといの土曜日に katsyoshi.org をのぞいたら、 *nginx* の初期ページが表示されたので
とくにコンテンツはないですが、いそいでサイトの復旧をしました。
が、もともとあったファイル置場を失念したため似た感じで再構築してました。
再構築ついでにssl化、`nginx` から `h2o` へのWebサーバー変更しました。
あと `systemd` でデーモン化とかも。

## Let's Encrypt

リリースされてだいぶたつのですが、[Let's Encrypt][letsencrypt]を利用してみました。
Ubuntu Linux 16.04 では、簡単に導入できます。

```bash
$ sudo apt install letsencrypt
$ sudo letsencrypt certonly
```

でいくつかの質問に答えればおわりです。
ここで、戸惑った場面としては認証を受けたいサーバーの確認があるのですが、
サーバーポート443を開ける[必要があり][letsencrypt-jp]、1回失敗しました。

## H2O

せっかくだし、[H2O][h2o/h2o]を使おうと思います。
インストールはかんたんでいかのようにすればokです。

```bash
$ git clone h2o/h2o
$ cd h2o
$ cmake -DCMAKE_INSTALL_PREFIX_PATH=/opt/local .
$ make && make install
```

設定は[ここ][h2o/config]と[ここ][h2o/redirect]を参考にして以下にしています。

```
user: www-data
hosts:
  "katsyoshi.org:80":
    listen:
      port: 80
    paths:
      /:
        redirect: https://katsyoshi.org
hosts:
  "katsyoshi.org:443":
    listen:
      port: 443
      ssl:
        certificate-file: /etc/letsencrypt/live/katsyoshi.org/fullchain.pem
        key-file: /etc/letsencrypt/live/katsyoshi.org/privkey.pem
    paths:
      /:
        file.dir: /opt/website/top
access-log: /var/log/h2o/access.log
error-log: /var/log/h2o/error.log
pid-file: /var/run/h2o.pid
http2-reprioritize-blocking-assets: ON
```

## [h2o.service][h2o/systemd]

さいきんわだいのしすてむでーってやつででーもん？かしようとおもいます

```
[Unit]
Description=H2O the optimized HTTP/1, HTTP/2 server
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/var/run/h2o.pid
ExecStartPre=/opt/local/bin/h2o -c /etc/h2o/h2o.conf -t
ExecStart=/opt/local/bin/h2o -c /etc/h2o/h2o.conf -m daemon
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

いやーしすてむでーってべんりですねー

## owari
サイトがぶっとんだのでサイトの復旧？と let's encrypt でのSSL化、 h2oへのウェブサーバー変更と `systemd` でのデーモン化をやりました。

1年ぶりの日記だたので、Markdownどうかくんだっけ？とか、別のところで大変でした。

## 参考サイト

[letsencrypt]: https://letsencrypt.org
[letsencrypt-jp]: https://letsencrypt.jp
[h2o/h2o]: https://h2o.examp1e.net/
[h2o/config]: https://h2o.examp1e.net/configure/quick_start.html
[h2o/redirect]: https://github.com/h2o/h2o/wiki/redirect-HTTP-to-HTTPS
[h2o/systemd]: https://negima.mobi/2015/10/2092
