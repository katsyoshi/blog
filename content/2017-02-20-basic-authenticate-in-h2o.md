+++
path = "/blog/2017/02/20/basic-authenticate-in-h2o"
layout = "post"
title = "basic authenticate in h2o"
date = 2017-02-20
comments = true
categories = "diary h2o basic-authenticate"
+++

H2Oでベーシック認証したい場合は以下のようにします[^h2odoc]

```yaml
paths:
  "/":
    mruby.handler: |
      require "htpasswd.rb"
      Htpasswd.new("/etc/h2o/.htpasswd", "realm-name")
```

また、 `.htpasswd` で plain フォーマットはサポートされていません。

```console
failed to validate password using file:/etc/h2o/.htpasswd:crypt-style password hash is not supported
```

`.htpasswd` を手動で作成したい場合は以下の方法でできます[^apachdoc]

```ruby
require "digest/sha1"
require "base64"
open("/etc/h2o/.htpasswd", "w") do |w|
  w.write("user:{SHA}#{Base64.encode64(Digest::SHA1.digest("password"))}")
end
```

[^h2odoc]: Configure > Using Basic Authentication, DeNA Co., Ltd. et al., https://h2o.examp1e.net/configure/basic_auth.html, 2017/02/20 閲覧
[^apachdoc]: Password Formats, The Apache Software Foundation., https://httpd.apache.org/docs/2.4/misc/password_encryptions.html, 2017/02/20 閲覧
