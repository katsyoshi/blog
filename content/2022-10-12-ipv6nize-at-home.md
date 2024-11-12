+++
path = "/blog/2022/10/12/ipv6nize-at-home"
layout = "post"
title = "Communicate on IPv6 from home"
date = 2022-10-12
comments = true
categories = "tech diary"
+++

家のネットワークからインターネットへ出るとき `IPoE` を使ってたけど、家庭内 LAN のネットワークは `IPv6` を off にしていた。
この LAN 内 `IPv6` 化していなかった理由としては、昔 Linux (だけじゃないかも)が `IPv6` が利用できる状態だと
先に `IPv6` で繋ぎにいこうとして `IPv6` で通信できなかったら `IPv4` にフォールバックするという挙動で、すごくストレスフルだった。
この挙動の対処として、カーネルレベルで `IPv6` を off にしてました。
そうこうしているうちに契約している ISP が `IPv6` オプションなしで `IPoE` を利用できるようになったので、
とりあえず `IPoE` だけを利用できるようにして、家庭内の LAN はそのままという状態にしていました。
いいかげんこの家庭内 LAN のネットワークを `IPv6` 化し、インターネットと `IPv6` で通信できるようにした顛末をのこす。のこします。


## 家庭内 LAN 環境

- ゲートウェイ: NEC IP38X/1210 (YAMAHA RTX1200)
- PC: 6.0.0-gentoo

![](/images/home2internet.png)

## IPoE 化と家庭内 LAN の IPv6 化

設定は [YAMAHA の設定例集](https://network.yamaha.com/setting/router_firewall/flets/flets_other_service/ipv6_ipoe)に載ってあるのでそれを参考にします。
`IPoE` 化はこの設定例でいけるのですが、どうも設定したときにミスったらしく、 LAN の IP アドレスが `IPv4` ではインターネットへ出ることができるが、
`IPv6` ではインターネットへ出ることができない状態になってしまいました。

### 旧設定
どのような設定だったかは以下に

```
ip route default gateway tunnel 1
ipv6 prefix 1 ra-prefix@lan2::/64
ipv6 lan1 address ra-prefix@lan2::1/64
ipv6 lan1 prefix ra-prefix@lan2::/64
ipv6 lan1 rtadv send 1
ipv6 lan1 dhcp service server
ipv6 lan2 address auto
ipv6 lan2 secure filter in 1010 1011 1012 2000
ipv6 lan2 secure filter out 3000 dynamic 100 101 102 103 104 105 106
ipv6 lan2 dhcp service client ir=on
ngn type lan2 ntt
tunnel select 1
 tunnel encapsulation ipip
 tunnel endpoint address 2404:8e00::feed:100
 tunnel enable 1
ipv6 filter 1010 pass * * icmp6 * *
ipv6 filter 1011 pass * * tcp * ident
ipv6 filter 1012 pass * * udp * 546
ipv6 filter 2000 reject * * * * *
ipv6 filter 3000 pass * * * * *
ipv6 filter dynamic 100 * * ftp
ipv6 filter dynamic 101 * * domain
ipv6 filter dynamic 102 * * www
ipv6 filter dynamic 103 * * smtp
ipv6 filter dynamic 104 * * pop3
ipv6 filter dynamic 105 * * tcp
ipv6 filter dynamic 106 * * udp
```

この設定ではインターネットとは `IPv4` でしか通信できていない状態でした。
でこのトラブルシュートとして知人([@n_kane](https://twitter.com/n_kane), [@paina](https://twitter.com/paina)) のちからを借りてどこまで通じてどこから通じないかを確認しました。

### troubleshooting

とりあえず `ping6` 、 `traceroute6` で通じていないことを確認します。

```
$ ping6 -c 3 google.com

PING  google.com(nrt13s55-in-x0e.1e100.net (2404:6800:4004:824::200e)) 56 データ長(byte)

--- google.com ping 統計 ---
送信パケット数 3, 受信パケット数 0, パケット損失 100%, 時間 2067ミリ秒

$ traceroute6 google.com
traceroute to google.com, 30 hops max, 80 byte packets
 1  * * *
 2  * * *
 3  * * *
 4  * * *
 5  * * *
 6  * * *
 7  * * *
 8  * * *
 9  * * *
10  * * *
11  * * *
12  * * *
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  * * *
22  * * *
23  * * *
24  * * *
25  * * *
26  * * *
27  * * *
28  * * *
29  * * *
30  * * *
```

通ってないですね。
先述したように PC は `IPv6` を on にしたばかりなのでクライアント側の設定でブロックしていないかを確認します。
まずクライアント側で考えられるのは `iptables` でのパケットフィルタリングですね。
ルーターの下にある PC なので、ここでブロックすることは低いのですが、確認します。

```
# ip6tables --list
Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```

なにもフィルタリングしていないですね。
ということで、クライアント側には問題なさそうですね。
どこまで通じてどこまで通じていないのか別のエンドポイントで確認します。

まず、手元と相手で `tcpdump` 利用してパケットが送っているか届いているかを確認します。

```
# tcpdump -nei enp8s0f0 icmp6
dropped privs to pcap
tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
listening on enp8s0f0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
22:35:41.158558 00:a0:de:69:40:9d > 33:33:ff:51:5e:63, ethertype IPv6 (0x86dd), length 86: fe80::212:e2ff:fe70:6144 > ff02::1:ff51:5e63: ICMP6, neighbor solicitation, who has 2409:10:a5c0:1f00:d903:ddb5:851:5e63, length 32
22:35:58.765783 ee:60:51:38:43:9c > 33:33:ff:00:00:01, ethertype IPv6 (0x86dd), length 86: 2409:10:a5c0:1f00:5ce3:afcb:9bb2:5c1b > ff02::1:ff00:1: ICMP6, neighbor solicitation, who has 2409:10:a5c0:1f00::1, length 32
22:36:09.249427 ee:60:51:38:43:9c > 33:33:ff:00:00:01, ethertype IPv6 (0x86dd), length 86: 2409:10:a5c0:1f00:5ce3:afcb:9bb2:5c1b > ff02::1:ff00:1: ICMP6, neighbor solicitation, who has 2409:10:a5c0:1f00::1, length 32
```

送れてはいるようで対向側にも届いていたらしく戻りのパケットは送ってたようですが、手元では戻ってくるパケットは見えていないですね。

![](/images/ping6-failure.png)

ということはやはりルーターの設定が悪そうということが推察されますね。
設定のどこが悪いのかあやしいところを見ていきます。

最初に怪しいとおもったのはルーターのフィルターまわりです。一旦、`no ipv6 interface secure filer in` で全部無効化しましょう。しかしとくに変りはないです。これはフィルターが原因ではなさそうですね。

次にあやしい点は以下2つの設定

1. [`ipv6 interface address`](http://www.rtpro.yamaha.co.jp/RT/manual/rt-common/ipv6/ipv6_interface_address.html)
1. [`ipv6 interface prefix`](http://www.rtpro.yamaha.co.jp/RT/manual/rt-common/ipv6/ipv6_interface_address.html)

上のコマンドは ISP から自動で振ってきている IP を割り当てる設定で、下のコマンドは ISP から振ってきた `IPv6` のプレフィックスを付けるようにするための設定となります。
どうもこの下のコマンドが邪魔なようです。
`no ipv6 interface prefix` で無効化できるので一旦外してみましょう。
すると

```
$ ping6 -c 3 google.com
PING google.com(nrt20s18-in-x0e.1e100.net (2404:6800:4004:81c::200e)) 56 データ長(byte)
64 バイト応答 送信元 nrt20s18-in-x0e.1e100.net (2404:6800:4004:81c::200e): icmp_seq=1 ttl=57 時間=6.53ミリ秒
64 バイト応答 送信元 nrt20s18-in-x0e.1e100.net (2404:6800:4004:81c::200e): icmp_seq=2 ttl=57 時間=2.73ミリ秒
64 バイト応答 送信元 nrt20s18-in-x0e.1e100.net (2404:6800:4004:81c::200e): icmp_seq=3 ttl=57 時間=2.81ミリ秒

--- google.com ping 統計 ---
送信パケット数 3, 受信パケット数 3, パケット損失 0%, 時間 2002ミリ秒
rtt 最小/平均/最大/mdev = 2.731/4.023/6.532/1.773ミリ秒
```

のように通りました!勝ったッ!第三部完!

## おわり

以前中途半端に設定したおかげで `IPv6` 有効化に時間が掛ってしまった顛末をまとめました。
これで家のネットワークから `IPv6` で通信できるようになりました。
ありがとうございました!

次回 katsyoshi.org お名前.COM やめるってよ
