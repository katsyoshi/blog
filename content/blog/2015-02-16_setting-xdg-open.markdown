+++

title = "XDGの設定"
date = 2015-02-16
comments = true
categories = "tech linux xorg"
+++

急に `xdg-open` でブラウザが開かなくなったのでメモ

```sh
$ xdg-mime default google-chrome.desktop x-scheme-handler/http
$ xdg-mime default google-chrome.desktop x-scheme-handler/https
```

で設定できます。

### 参考

'xdg-open', archlinux wiki, https://wiki.archlinux.org/index.php/xdg-open, 2015/02/16
