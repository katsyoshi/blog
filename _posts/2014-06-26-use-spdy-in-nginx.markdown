---
layout: post
title: SPDYをさくっと動かしてみた
date: 2014-06-26 13:59:19 +0900
comments: true
categories: nginx http2 tech
---

NGINXでSPDY3.1が[サポートされている](http://nginx.org/en/docs/http/ngx_http_spdy_module.html)ようなので動かしてみた。

## 準備
SPDYは[SSL必須](http://ja.wikipedia.org/wiki/SPDY#.E6.A6.82.E8.A6.81)のようなのでとりあえずOpenSSLで[野良証明書を作成](http://dogmap.jp/2011/05/10/nginx-ssl/)します。

```
$ cd /path/to/cert/dir
$ openssl genrsa -des3 -out server.key 2048
$ openssl req -new -key server.key -out server.csr
$ openssl rsa -in server.key.org -out server.key
$ openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```

## NGINXの設定

とりあえずNGINXの[設定ファイルを作成](http://nginx.org/en/docs/http/ngx_http_spdy_module.html)します。

```
server {
    listen 443 ssl spdy;
    ssl_certificate server.crt;
    ssl_certificate_key server.key;
}
```

と設定したらNGINXを再起動します。
これでSPDYで動いてます。わいわい。

![SPDY](/images/photo/spdy.png)

オレオレ証明書なのでそのうちちゃんとした証明書を使いたいですね。
