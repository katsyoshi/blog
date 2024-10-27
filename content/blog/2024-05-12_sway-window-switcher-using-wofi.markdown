+++

title = "Sway 用 Window 切り替えを作った"
date = 2024-05-11
comments = true
categories = "diary tech"
+++

あたらしく PC 買って、 **Linux** の GUI 環境を **X11** から **Wayland** に乗り変えることにしました。
いままで利用していたデスクトップ環境を [**i3**](https://i3wm.org/) から [**sway**](https://swaywm.org/)
に変更しました。
最近利用していた **macOS** や **i3** ではあまり気にしていなかったのですが、**Windows** の `Alt+Tab` での
ウインドウ切り替えが便利だったのを思い出したのでこの便利な機能を模倣することにしました。

でも本当にほしいのは `Alt+Tab` の順番に切り変わるやつではなく、アクセスしたいウィンドウへの切り変えなのです。
そこでこの機能を **ruby** で実装することにしました。

## 準備

準備として以下のソフトウェアがインストールされていることを期待しています。

- window manager: **i3**/**sway**
- launcher/menu program: [**wofi**](https://hg.sr.ht/~scoopta/wofi)

## 作成

なにかしら実装あるだろうということで参考を探していたら[見付けた](https://gist.github.com/muniter/1c187e7c973accba463fb05c1402621f)のでその実装を参考にします。
プログラムは以下になります。

```ruby
#!/usr/bin/env ruby

require 'bundler/inline'

gemfile do
  source "https://rubygems.org"
  gem "i3ipc"
end

require "i3ipc"
require "open3"

class SwayWindowSwithcer
  def self.switch = new.switch
  def initialize
    @conn = I3Ipc::Connection.new
    @workspaces = @conn.workspaces
    @windows = Set.new
    set_windows
  end
  def switch = @conn.command("[con_id=#{@windows.to_a[open].to_h[:id]}] focus")

  private
  def list_window = @windows.map(&:name)
  def displays = @conn.tree.nodes.reject { |display| display.name == "__i3" }
  def open
    Open3.popen3(['wofi', '-i', '-k', '/dev/null', '-d'].join " ") do |i, o, _e, _w|
      i.puts list_window.join("\n")
      i.close
      list_window.index(o.read.strip)
    end
  end

  def set_windows
    displays.map do |display|
      display.nodes.map { |workspace| @windows += workspace.nodes }
    end
  end
end

SwayWindowSwithcer.switch
```

**i3** は [**IPC** が定義](https://i3wm.org/docs/ipc.html) されています [^ipc] 。
今利用している **sway** は **i3** の **wayland** 向け実装なので **IPC** に互換性があります。
そのまま **i3ipc** 関連のライブラリを利用することが可能です。
ということで今回は **ruby** の **gem** として **i3ipc.gem** というそのままのやつがあったので利用します。
**i3** で起動しているウィンドウ一覧を取得して、名前を **wofi** に渡します。
ウィンドウ一覧を受け取った **wofi** は切り替えたいウィンドウを絞り込み、選択ができます。
切り替えたいウィンドウを選択したら、今度は **i3** へ選択したウィンドウへのフォーカスする命令を送ります。

ここで注意点として、二つありひとつ目は以下があります。
**ruby** の **i3ipc** は `I3Ipc::Connection` からでしかコマンドを送れないです[^sanko]。
そのためこのプログラムでは `@conn.command` でコマンドを送るとします。
このままではどのウインドウかはわからないので **i3** コマンドを送る際に `[con_id=id]` を付けます。
今回はウィンドウを切り替えることをしたいだけなので `[con_id=id] focus` とします。
ふたつ目はシステムの **ruby** を利用する際には必要な **gem** がシステムにインストールされている必要があります。
もしインストールされていないのであれば手動でいれる必要があります[^bundler]。

![](/images/screenshot/wofi-window-switcher.png)

## おわり

あたらしい PC を買い、デスクトップ環境を替えました。
あたらしい環境で少し不便だったところを解消するプログラムを書いてみました。

+++

[^ipc]: InerProcess Communication
[^sanko]: 参考にした **Python** はウィンドウのオブジェクトから直接コマンドが送れます。
[^bundler]: **root** ユーザーで一度実行するか、手動でやる必要がある。インラインとはいったい……
