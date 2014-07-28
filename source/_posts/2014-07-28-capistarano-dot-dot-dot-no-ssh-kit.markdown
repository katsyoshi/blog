---
layout: post
title: "Capistranoかと思ったらSSHKitの件"
date: 2014-07-28 22:14:09 +0900
comments: true
categories: tech capistrano server
---
JubatusをCapistrano使って起動させようとして嵌ったのでメモ。環境はUbuntuです。

## JubatusをCapistranoで起動させる
まず、CapistranoでJubatusを起動させるスクリプトを追記します。

```rake
namespace :jubatus do
  desc 'start jubatus'
  task :start do
    on roles(:app), in: :sequence, wait: 5 do
      set :ld_library_path, '/opt/jubatus/lib'
      execute "LD_LIBRARY_PATH=#{fetch(:ld_library_path)} /opt/jubatus/bin/jubaclassifier -f /path/to/jubatus/config/classifier_config.json -p 9199 -l ${HOME}/jubatus/logs -D &"
    end
  end

  desc 'stop jubatus'
  task :stop do
    on roles(:app), in: :sequence, wait: 5 do
      execute :pkill, 'jubaclassifier'
    end
  end
end
```

以下のコマンドを入力します。

```
$ bundle exec cap development jubatus:start
```

