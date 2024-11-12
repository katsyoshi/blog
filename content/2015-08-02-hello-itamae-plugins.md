+++

title = "こんにちは、いたまえさん"
date = 2015-08-02
comments = true
categories = "tech itamae proxy rbenv plenv pyenv"
+++

[rbenv](https://github.com/sstephenson/rbenv) を [itamae](https://github.com/itamae-kitchen/itamae) の [プラグイン](https://github.com/k0kubun/itamae-plugin-recipe-rbenv) を利用してインストールしようとしたら、対象サーバがプロキシーのあるネットワークであり、gitでダウンロードできなかったので、[httpsを指定できる](https://github.com/k0kubun/itamae-plugin-recipe-rbenv/pull/9)ようしてもらいました。

## plenv と pyenv
また、rbenv のプラグインを参考に [plenv](https://github.com/katsyoshi/itamae-plugin-recipe-plenv) と [pyenv](https://github.com/katsyoshi/itamae-plugin-recipe-pyenv) を作ってみました。

これらふたつのプラグインはまだgemになっていないので、`Gemfile` に以下のように追加し、

```ruby
gem 'itamae'
gem 'itamae-plugin-recipe-plenv', github: 'katsyoshi/itamae-plugin-recipe-plenv'
gem 'itamae-plugin-recipe-pyenv', github: 'katsyoshi/itamae-plugin-recipe-pyenv'
```

recipeとしては以下のようにしてください。

```ruby recipe.rb
include_recipe 'pyenv::system'
include_recipe 'plenv::system'
```

設定例としては、rbenvのプラグインと同じように設定します。

```json
{
  "plenv" : {
    "versions" : ["5.22.0", "5.20.2"],
    "global"   : "5.22.0"
  },
  "pyenv" : {
    "versions" : ["3.4.3", "3.5.0b2"],
    "global"   : "3.4.3"
  }
}
```

## おわり

おわり
