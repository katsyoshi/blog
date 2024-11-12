+++

title = "Hello, Wezterm"
date = 2022-03-19
comments = true
categories = "diary"
+++

[`tmux`](https://github.com/tmux/tmux) + [`Allacritty`](https://github.com/tmux/tmux) が疲れてきたので[はてぶで流れてきた](https://zenn.dev/yutakatay/articles/wezterm-intro) [`wezterm`](https://github.com/wez/wezterm) が [`sixel`](https://github.com/saitoha/libsixel) を利用できてよさそうだったので試してみることにした。

## 設定

設定ファイルが [lua](https://www.lua.org/) でカスタマイズがいろいろとできるのでまずは色を代えてみます。

```lua
local wezterm = require 'wezterm'
return {
  color_scheme = "Dracula",
}
```

プログラミング言語でカスタマイズができるので以下のようにアクティブなタブへの移動のキーバインドのカスタマイズができます。

```lua
local wezterm = require 'wezterm'
local move_keys = {}

for i = 1, 9 do
   table.insert(move_keys, {
      key = tostring(i),
      mods = "CTRL",
      action = wezterm.action{ ActivateTab = i - 1, },
   })
end

return {
   color_scheme = "Dracula",
   disable_default_key_bindings = true, -- 初期のキーバインドは利用しない場合
   keys = move_keys,
}
```

こんな感じで設定できるので便利です。

このキーバインドは任意のイベントも設定でき、任意のイベントを利用してアクションを定義できます。以下の例では、Paneを開い監視用のプログラムを開きます。

```lua
local wezterm = require 'wezterm'
wezterm.on("open-nvtop-and-ytop", function(win, pane)
   win:perform_action(wezterm.action{ SplitHorizontal = { domain = "CurrentPaneDomain", args = { "nvtop", }, }, }, pane)
   win:perform_action(wezterm.action{ SplitVertical = { domain = "CurrentPaneDomain", args = { "ytop", "-ps", }, }, }, pane)
end)

return {
   keys = { { key = "r", mods = "CTRL", action = wezterm.action{ EmitEvent = "open-nvtop-and-ytop", }, }, },
}
```

とすると以下のようになります。
<img src="/images/screenshot/open-nvtop-and-ytop-in-wezterm.png" width="100%">
便利!

こんな便利なものということで `systemd` でデーモン化しています。

```toml
[Unit]
Description=GUI Accellarated terminal
Documentation=

[Service]
Type=forking
ExecStart=/usr/local/bin/wezterm-mux-server --daemonize
Restart=on-failure

[Install]
WantedBy=default.target
```

で起動しておいています

### 問題点

と設定ファイルの例書いてみたのですが、とても大きな問題点にブチあたったので書いておきます。

`wezterm` には `wezterm-mux-server` というマルチプレクサ(tmuxのように扱うため)のサーバーモードプログラムがあるのですが、こいつがどうも `wezterm` とは挙動が異なり、前述した監視用のキーバインドが微妙に異なった挙動となってしまっています。サーバーモードに接続した場合の挙動は以下のようになります。

<img src="/images/screenshot/failed-nvtop-ytop.png" width="100%">

1つ目はpaneの位置が期待したとおりになっていない。2つ目は `ytop` が起動していないというので2つ目の方は気にしなければいいのでまあいいかと思っている。1つ目の問題は許容できていないので一旦はこのキーバインドは封印となっています。

# おわり

長年利用してた `tmux` を捨てて `wezterm` を利用しはじめた。
設定が `lua` で書けるのが体験的にとても良いのでこれからも利用するかなと。
`wezterm` で `sixel` 利用した画像表示ができるようになったのが便利なので「よしっ！」

