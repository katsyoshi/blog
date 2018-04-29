---
layout: post
title: "アクセスログをTreasureData.comへ"
date: 2014-04-11 00:50:27 +0900
comments: true
categories: linux tech log fluentd
---

いいかげん[treasure data](http://www.treasuredata.com)を使おうかな。
ということでこの鯖のアクセスログをfluentdを使って保存しようとおもいます。

今回は[td-agent](https://github.com/treasure-data/td-agent)を利用せずにgemからインストールしたfluentdを利用します。

## 環境
環境としてUbuntu 12.04を利用しています。

## 準備
事前準備としてユーザ、グループの作成、rubyのインストールを行ないます。

### ユーザ、グループの作成
以下のコマンドでユーザ、グループの作成を行います。
```
$ sudo adduser fluentd -s /bin/false
```
指示に従って入力するとユーザ、グループが作成されています。
このユーザは[ログインできません](http://qiita.com/shunichi/items/c7744878f5c02eaab18d#2-5)。

### Rubyのインストール

以下のコマンドを入力しRubyのインストールを行ないます。

```
$ sudo aptitude build-dep ruby1.9.3
$ sudo aptitude install git
$ sudo git clone git://github.com/sstephenson/rbenv.git /usr/local/rbenv
$ sudo git clone git://github.com/sstephenson/ruby-build.git /usr/local/rbenv/plugins/ruby-build
$ export RBENV_ROOT=/usr/local/rbenv
$ export PATH=${RBENV_ROOT}/bin:${PATH}
$ eval "$(rbenv init -)"
```

ここまで入力したら`sudo visudo -f /etc/sudoers.d/00_base`と入力し、以下を[入力してください](http://office.tsukuba-bunko.org/ppoi/entry/systemwide-rbenv)。

```
Defaults !secure_path
Defaults env_keep += "PATH RBENV_ROOT"
```

入力したらRuby 2.1.1をインストールします。

```
$ sudo rbenv install 2.1.1
$ sudo rbenv rehash
```
でインストール完了です。次にfluentdをインストールします。

### fluentdとプラグインのインストール
gemでfluentdとtdプラグインのfluent-plugin-tdをインストールします。

```
$ sudo gem install fluentd fluent-plugin-td
```

つぎに設定ファイル`fluentd.conf`を作成します。

```xml fluentd.conf
<source>
  type forward
</source>

<source>
  type tail
  path /var/log/nginx/access.log
  format nginx
  time_format %d/%b/%Y:%H:%M:%S %z
  tag td.nginx.main.access
  pos_file /var/log/fluentd/main_access.pos
</source>

<match td.**.*>
  type copy
  <store>
    type stdout
  </store>
  <store>
    type tdlog
    endpoint api.treasure-data.com
    apikey ここにtdのAPIキーを入力してね
    auto_create_table
    buffer_type file
    buffer_path /var/log/fluentd/buffer/td
    use_ssl true
  </store>
</match>
```

作成後起動確認を行なってください `fluentd -c fluentd.conf` 。
起動確認を行ったら `fluentd.conf` を `/etc/fluentd/fluentd.conf` に移動します。
これで終了です。

### init.dスクリプトの作成
まず `/etc/init.d/skelton` を `/etc/init.d/fluentd` にコピーします。
コピーしたら以下の様にします。

```diff fluentd_diff_skelton
diff --git a/etc/init.d/skeleton b/fluentd
old mode 100644
new mode 100755
index dac9480..c59505e
--- a/etc/init.d/skeleton
+++ b/fluentd
@@ -1,6 +1,6 @@
 #! /bin/sh
 ### BEGIN INIT INFO
-# Provides:          skeleton
+# Provides:          fleuntd
 # Required-Start:    $remote_fs $syslog
 # Required-Stop:     $remote_fs $syslog
 # Default-Start:     2 3 4 5
@@ -19,19 +19,14 @@

 # PATH should only include /usr/* if it runs after the mountnfs.sh script
 PATH=/sbin:/usr/sbin:/bin:/usr/bin
-DESC="Description of the service"
-NAME=daemonexecutablename
-DAEMON=/usr/sbin/$NAME
-DAEMON_ARGS="--options args"
-PIDFILE=/var/run/$NAME.pid
-SCRIPTNAME=/etc/init.d/$NAME
-
-# Exit if the package is not installed
-[ -x "$DAEMON" ] || exit 0
+NAME=fluentd

 # Read configuration variable file if it is present
 [ -r /etc/default/$NAME ] && . /etc/default/$NAME

+# Exit if the package is not installed
+[ -x "$DAEMON" ] || exit 0
+
 # Load the VERBOSE setting and other rcS variables
 . /lib/init/vars.sh

@@ -49,10 +44,11 @@ do_start()
  #   0 if daemon has been started
  #   1 if daemon was already running
  #   2 if daemon could not be started
-    start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON --test > /dev/null \
+    start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON \
+    ${START_STOP_DAEMON_ARGS} --test > /dev/null \
      || return 1
-    start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON -- \
-        $DAEMON_ARGS \
+    start-stop-daemon --start --quiet --pidfile $PIDFILE --exec $DAEMON \
+        ${START_STOP_DAEMON_ARGS} -- $DAEMON_ARGS \
      || return 2
  # Add code here, if necessary, that waits for the process to be ready
  # to handle requests from services started subsequently which depend
```

このままでは起動しないので `/etc/default/fluentd` を作成します。

```sh fluentd.default
RBENV_ROOT=/usr/local/rbenv
PATH=${RBENV_ROOT}/bin:${PATH}
eval "$(rbenv init -)"
USER=fluentd
GROUP=fluentd
DESC="fluentd"
PIDFILE=/var/log/$NAME/run.pid
CONFFILE=/etc/fluentd/fluentd.conf
DAEMON=/usr/local/rbenv/shims/fluentd
DAEMON_ARGS="--daemon $PIDFILE --log /var/log/fluentd/fluentd.log --config $CONFFILE"
SCRIPTNAME=/etc/init.d/$NAME
START_STOP_DAEMON_ARGS=""

if [ -n "${USER}" ]; then
   if ! getent passwd | grep -q "^${USER}:"; then
      echo "$0: user for running td-agent doesn't exist: ${USER}" >&2
      exit 1
   fi
   if [ ! -d $(dirname ${PIDFILE}) ]; then
       mkdir -p $(dirname ${PIDFILE})
   fi
   chown -R ${USER} $(dirname ${PIDFILE})
   START_STOP_DAEMON_ARGS="${START_STOP_DAEMON_ARGS} -c ${USER}"
fi

if [ -n "${GROUP}" ]; then
   if ! getent group | grep -q "^${GROUP}:"; then
       echo "$0: group for running td-agent doesn't exist: ${GROUP}" >&2
       exit 1
   fi
   START_STOP_DAEMON_ARGS="${START_STOP_DAEMON_ARGS} --group ${GROUP}"
fi
```

としたら、以下のコマンドを入力し、fluentdデーモンを起動します。

```
$ sudo update-rc.d fluentd defaults
$ sudo mkdir -p /var/log/fluentd
$ sudo chown fluentd:fluentd /var/log/fluentd
$ sudo service fluentd start
```

で起動してるはずでう。

## おわり

いくつかアクセスしてみてなげられてるのを確認できたらねます。
最後に[さけまつり](http://katsyoshi.doorkeeper.jp/events/10420)やるかもしれないのできてみてくだしあ。


