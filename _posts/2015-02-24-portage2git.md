---
layout: post
title: "emerge --syncをgitにかえたはなし"
date: 2015-02-24 00:13:18 +0900
comments: true
categories: linux gentoo tech
---

portageはデフォルトだと `rsync` を利用しての更新をしているので、
`git` を用いた方法に変更します。

## 準備

`portage` を `2.2.17` に変更し、USEフラグとして `git` をつかいます。

```
# USE="git ${USE}" emerge -av \=portage-2.2.17
```

つぎに、Gitのリポジトリを見に行くように[設定](http://qiita.com/wjn/items/a3b71059211711501e35)します。

と設定してあとはいつもどおり `sudo eix-sync` させるだけで大丈夫です。

## おわり

基本的にはリンク先だけ見てればいけると思います。

引越ししてWiMAXの電波の入りがわるくなったので鉄製ボウル買おうかとおもってたら2時間ほど[徘徊](https://ingress.com)してました。

### 参考サイト

1. http://qiita.com/wjn/items/a3b71059211711501e35
2. https://wiki.gentoo.org/wiki/Project:Portage/Sync
