+++

title = "Emacsの構成管理をitamaeで管理"
date = 2017-07-01
comments = true
categories = "diary itamae"
+++

仕事とプライベート環境でemacsのフォント等設定するのがいいかげんダるくなってきたので、Itamaeを流すだけでイイカンジにするようにした

## もともとどうかんりしてたのか
もともと[github](https://github.com/katsyoshi/dot.emacs.d)で管理していたが、さすがにFontの環境異差をちょこちょこ変えるのが面倒になった

## Itamae de kanri
gitで管理しているので環境異差ある部分を[itamae](https://github.com/katsyoshi/itamae-recipes)で管理するように方針を転換。

```yaml
emacs:
  font:
    family: Ricty
    height: 120
  packages:
    - auto-complete
  settings:
    - git
```

とか書いてあとはItamaeを実行することで必要なパッケージのインストール、
必要な設定へのリンク追加などをするようにしました。

## おわり
これでなにも考えなくてすむようになるのかな？
