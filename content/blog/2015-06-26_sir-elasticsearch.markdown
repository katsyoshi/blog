+++
layout: post
title: "elasticsearchどの〜"
date: 2015-06-26 00:26:10 +0900
comments: true
categories: elasticsearch fluentd kibana
+++

[kibana](https://www.elastic.co/products/kibana) で表示しようとしてたら嵌ったのでメモ

## 嵌った原因
数字をElasticsearchに投げているつもりが文字列を投げていた。
ので対処方法としては、 `fluent-plugin-typecast` を導入して終了。

```xml
<match elasticsearch.**.*>
  type typecast
  item_types value:float
  prefix typed
</match>
```

## Elasticsearch

[fluentd](https://www.fluentd.org) で集めたデータを [Elasticsearch](http://www.elasitc.co) に [fluent-plugin-elastchsearch](https://github.com/uken/fluent-plugin-elasticsearch) を利用して入れます。設定は以下のようにします。

```xml
<match typed.elasticserch.**.*>
  type elasticsearch
  type_name hoge
  host 127.0.0.1
  port 9200
  logstash_format true
  logstash_prefix logstash
  flash_interval 1s
</match>
```

## おわり
おわり

### 参考資料
1. http://blog.nomadscafe.jp/2014/03/dstat-fluentd-elasticsearch-kibana.html
1. https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-core-types.html
