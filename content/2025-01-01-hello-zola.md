+++
title = "Hello, ZOLA"
date = 2025-01-01T00:00:00+09:00
path = "/blog/2025/01/01/hello-zola"
+++

HELLO, [ZOLA](https://www.getzola.org) World!!!!!

## ELECTIONS

_[Jekyll](http://jekyllrb.com/)_ でサイト作るの飽きてきたので、
新しい _SSG(Static Site Generator)_ に変更することにした。

選定必須基準として、以下の条件を。

1. _[markdown 形式](https://tex2e.github.io/rfc-translater/html/rfc7763.html)_[^markdown] で書けること
1. **Jekyll** と同じ _URL_ が生成されること

いまどき直接 _HTML_ で書く人はあまりいないし、いままでの記事があるので _markdown_ で書けることは当然ですね。
もうひとつの必須条件として、 **Jekyll** で設定した _URL_ で記事が書けることとなります。
これは現在出している記事の _URL_ を変更しないために必須です。

移行に苦労しないようにするために以下の条件をとることにした。

1. **Jekyll** と同じ様に _markdown_ の先頭に _frontmatter_ でいろいろと設定できること
1. **Jekyll** と同様にテーマが選択できること
1. バイナリーで配ってること

_frontmatter_ については移行を楽にするためで多少の書き替えはしかたがないとしています。
デザイン変更を楽にするためテーマが選択できる _SSG_ だといいなあと思い。
バイナリーで配ってあると楽なんですよ[^binary]。

以上の条件を踏まえいくつか検討してみましたが、 **zola** にしました。

[![getzola/zola - GitHub](https://gh-card.dev/repos/getzola/zola.svg)](https://github.com/getzola/zola)

## Jekyll to Zola

[![katsyoshi/jekyll2zola - GitHub](https://gh-card.dev/repos/katsyoshi/jekyll2zola.svg)](https://github.com/katsyoshi/jekyll2zola)

### やったこと

まず、ファイル名が `.markdown` と混在してので `.md` へ統一。
つぎに _markdown_ 先頭の _frontmatter_ を [**YAML**](https://yaml.org) から [**TOML**](https://toml.io) へ変換する。
これは大変な話ではなく、 _frontmatter_ 部分を読み込んで変更するだけです。
でこの変更部分なのですが、大抵の部分は問題にならないです。
大抵の部分は……

#### _URL_ の設定

_URL_ の設定は、[参考サイト](https://zenn.dev/anz/scraps/ebf857a5cbcfb6#front-matter-%E3%81%A7%E5%88%B6%E5%BE%A1%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95) の設定を利用して変換します。
ここで問題となるのが、**Jekyll** の設定で、 `/blog/%Y/%m/%d/file-name` となっていることです。
~~**Zola** では `config.toml` で設定する `slugify` に設定する方法があるようですが、イマイチ使えず~~
（そもそも `slugify` を勘違いしてた）。
ディレクトリを利用し[設定する方法](https://www.getzola.org/documentation/content/overview/)と _frontmatter_ に `path` で[設定する方法](https://www.getzola.org/documentation/content/page/)があります。
今回は _frontmatter_ で `path` 指定して _URL_ を設定することにしました。
変換はファイル名から取得すればよいので問題は特になく変換できました。
ただ、変換する際にこのままだと挿入するには注意が必要で、挿入する必要があります。
これは [`Array#insert`](https://docs.ruby-lang.org/ja/latest/method/Array/i/insert.html) を利用すれば [解決](https://github.com/katsyoshi/jekyll2zola/blob/main/lib/jekyll2zola/converter.rb#L34) ですね。

#### テーマ

[テーマ](https://www.getzola.org/themes/) はあるのですがプラグイン形式というわけではなく、 `git clone` or `git submodule add` して利用する点が慣れない。
気になる部分は `template/` に修正ファイルを全部作る必要があり面倒でした。
[_Cargo_](https://crates.io/) でインストールできればいいのに……
ということでもっと面倒臭い自分のテーマを作ることでこの気になる点を解決。
作成方法は [公式ドキュメント](https://www.getzola.org/documentation/getting-started/overview/#templates) を参考にすすめれば問題なく作成できます。
テーマというか、普通にテンプレート化するだけです。
[テーマ化](https://www.getzola.org/documentation/themes/creating-a-theme/) も実際は簡単にでき、 `theme.toml` を書いておけば大丈夫なようです。
個人的に利用しているだけなのでテーマ化はしないですが。

#### FEED.XML

_[RSS Feed](https://www.rssboard.org/rss-specification)_ 自体はサポートされていますが、 _URL_ のパスが `/atom.xml` か `/rss.xml` になってしまうため
旧来のパス `/feed.xml` は新規にテンプレートを作る必要があります。
これは追い追い考える。現状 **Jekyll** にあった `feed.xml` を作るのは大変すぎる。

#### こまかい修正
いくつか気になる部分があったがこまかいので手で修正

1. 脚注の順番が脚注を書いた順に出力
1. _Syntax Highlighting_ のサポート言語
1. 画像の変更
1. _rake task_ の整理

脚注は脚注で引用した順番ではなく、脚注を書いた順になっているので修正してます。
_Syntax Highlighting_ ではサポートされていない書きかた（**console**）をやめてサポートされている言語（**bash**）へ修正。
画像の変更はとくに **zola** は関係なく、大きいファイルが多いので変更しています。 
_rake task_ では、いままでデプロイやら記事作成やらなんだかんだやっていたので整理を行いました。
今は一旦、記事作成だけにしております。そのうちデプロイを行なえるようになると思います

<hr>

[^markdown]: _[RFC](https://www.ietf.org/process/rfcs/)(Request For Comment)_ になっててびびってる
[^binary]: **Jekyll** は依存解決とかあって最新の [Ruby](https://www.ruby-lang.org) が使えなかったりしたのでストレスたかかった
