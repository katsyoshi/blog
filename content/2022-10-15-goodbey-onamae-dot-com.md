+++
path = "/blog/2022/10/15/goodbey-onamae-dot-com"
layout = "post"
title = "お名前ドットコムのメールがうざすぎたので DNS を Cloudflare に移行して快適生活"
date = 2022-10-15T23:59:50+09:00
comments = true
categories = "diary"
+++

**katsyoshi.org** の登録先を [**お名前ドットコム**](https://www.onamae.com) にしてたけど、広告のようなメールとか届くし
更新案内と広告の違いがわからない感じのメールが大量にくるのでやめようやめようと思ってたのでいいかげん変えてみた話。

## 準備

準備として移行先のレジストラを選定します。
移行先としては普通のレジストラとクラウド業者がやっているレジストラがあると思いますが、今回は以下3つを候補にしました。

1. [**Google Domains**](https://domains.google/intl/ja_jp/): **Google** がやっているやつ。メールとか **Google** なんで _DNS_ まで **Google** にするのは心理的抵抗が強い。
1. [**Route 53**](https://aws.amazon.com/jp/route53/): みんなつかってる **AWS** のサービス。仕事で利用しているので、プライベートは別のがいいかな。
1. [**Cloudflare の DNS**](https://www.cloudflare.com/ja-jp/dns/): みんなだいすき低価格 _CDN_ 業者の [**Cloudflare**](https://cloudflare.com) がやってる _DNS_ サービス。

**Google** 、 **AWS** は言わずと知れた巨大企業でサービスがなくなるということはないとおもうが、
仕事で利用したり、情報全部預けたりしているところなので選択する理由が個人的には弱い。
個人利用でガンガン変えたり、 **VPC** でネットワーク構築するわけじゃないので **Cloudflare** でいいかなと。

### お名前ドットコムでの作業

レジストラを移管する前に現在登録してある [WHOIS 情報](https://www.nic.ad.jp/ja/whois/) を確認します。
これは移管作業で移管作業用コードが WHOIS 登録者にメールが送られてくるのでどのメールアドレスかの確認です。
ここで[WHOIS 情報公開代行](https://www.onamae.com/service/d-regist/option.html) を利用している場合は、
WHOIS を一旦登録時のものに変更します[^org]。

## 移管

移管作業としては新規レジストラで[移管依頼](https://developers.cloudflare.com/registrar/get-started/transfer-domain-to-cloudflare/)を参考に移管依頼ページを開きます。
開いたら、旧サービスから移管コードの発行を行ないます[^onamae]。
移管コードを [**Cloudflare** 側で入力して](https://zenn.dev/a24k/articles/20220527-cloudflare-dns)しばし待機。

しばらくしたら、レコードが登録されるので完了です。

## おわり

ということで _DNS_ の登録を **Cloudflare** へ移管しました。
**お名前ドットコム** はあのメールさえなければ続けたのかもしれない。
が更新警告と普通のメールの違いがあまりにもわからないので捨てることにしました。
**Cloudflare** で不満があったらまた変更すると思いますが、快適な生活になりました(たぶん

---

[^org]: 前に移行しようとしたとき、**.org** のドメインは WHOIS 情報書き変えられなくて移行失敗。現在この制限はなくなったので移行。
[^onamae]: こいつが見つけにくく、お名前ドットコムからは見つけられずに[移管レポートブログ](https://www.tsukimi.net/domain_onamae_xdomain.html)から発見。
