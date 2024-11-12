+++
path = "/blog/2015/04/07/many-ssh-connection"
layout = "post"
title = "known_hostsに載っていないホストに接続確認しないようにする"
date = 2015-04-07
comments = true
categories = "tech ssh"
+++

昨日の[続き](/blog/2015/04/06/careless-miss/)で、 `lxc` で大量のサーバを立てて、
SSH接続すると、ホストごとに毎回接続確認が必要なので、確認せずに接続できるようにした。

## 解決方法

```
$ ssh -oStrictHostKeyChecking=no anywhere.example.com
```

で接続確認の `yes or no` が出てこなくなります。

## おわり
おわり。

そのうち `lxc` で100個ほどのインスタンスのベンチマークとりたい…
