+++
path = "/blog/2016/10/10/fxxking-ssh-permission"
layout = "post"
title = "ssh permission"
date = 2016-10-10T20:14:30+09:00
comments = true
categories = "ssh memo "
+++

またやらかしたので

## 経緯
サーバーにSSHログインが急にできなくなった

## 対処

```
chmod 744 ~/.ssh
```

## おわり
おわり
