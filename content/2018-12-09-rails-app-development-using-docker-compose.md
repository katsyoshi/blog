+++
path = "/blog/2018/12/09/rails-app-development-using-docker-compose"
layout = "post"
title = "docker-composeを利用してRailsアプリ開発を楽にしよう"
date = 2018-12-09T21:11:56+09:00
comments = true
categories = "rails tech docker"
+++

ていうのを[うなすけさん](https://twitter.com/yu_suke1994)に相談したら、[解決策を示してくれた](https://blog.unasuke.com/2018/run-rails-test-on-docker-compose/)のでそれを開発向けに変えてみた。
Dockerとdocker-composeはみなさんごぞんじだと思うので割愛します。

## はじめに

Railsアプリ用 Dockerfile を準備します。こちらは、元記事と同じで良いとおもいますが、元記事ではすべての条件を満たすために、不要な `DB` やミドルウェアのライブライをインストールしてますので必要なものだけにします。
このRailsアプリでは、`DB` として `postgres` を利用していますので関連のライブラリをインストールします。

```
FROM ruby:2.5.3-stretch

ARG NODE_MAJOR_VER=11
ARG BUNDLER_JOBS=4

RUN curl -sL https://deb.nodesource.com/setup_${NODE_MAJOR_VER}.x | bash - \
  && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
  && apt update && apt install --assume-yes \
    postgresql-contrib \
    libpq-dev \
    libxml2 \
    libxml2-dev \
    libxslt1-dev \
    git \
    make \
    nodejs \
    yarn \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /rails
COPY . .
RUN bundle install --jobs=${BUNDLER_JOBS}
RUN yarn install
```

同様に `docker-compose.yml` を必要なものだけにします。

```
version: '3'
services:
  rails:
    build: .
    command: /bin/bash
    environment:
      - NODE_MAJAR_VER=11
      - BUNDLER_JOBS=4
      - DB_HOST=postgres
    volumes:
      - ".:/rails"
    links:
      - postgres
    command: ["bundle", "exec", "rails", "s", "-b", "0.0.0.0"]
    ports:
      - "3000:3000"
  postgres:
    image: postgres:11.1-alpine
    ports:
      - "5432:5432"
```

こちらも `postgres` だけにします。

## おわり

あとは `docker-compose up rails` とし、実行することで見れるようになっています。
