---
layout: post
title: "update test environment of redis-py"
date: 2015-12-29 21:56
comments: true
categories: [patch]
---
2015年は Redis と戯れていた一年と言っても良い。[Redis Cluster](http://redis.io/topics/cluster-spec) を Production 環境に導入し、[redis-py-cluster](https://github.com/Grokzen/redis-py-cluster) に [READONLY 対応の patch を送った](https://github.com/Grokzen/redis-py-cluster/pull/76) りした。

Redis Cluster は Redis 3.0 で加わった新機能であり、もうじき 3.2 が出ようとしている。そんな状態の中で 2.8 でしかテストされていないのはどうなの？ と思い、以下の 2つの patch を送っておいた。

* redis-py のビルド環境を 3.0 対応にする
    * [https://github.com/andymccurdy/redis-py/pull/697](https://github.com/andymccurdy/redis-py/pull/697)  
* RESTORE コマンドに 3.0 で加わった REPLACE修飾子に対応
    * [https://github.com/andymccurdy/redis-py/pull/698](https://github.com/andymccurdy/redis-py/pull/698)

特に前者は、2.8 によるテスト環境をごっそり 3.0 に入れ替えるものなので、割とドラスティックである。Redis 側で 2.8 をいつまでサポートするのか、というポリシーが明確ではないので、取り込むタイミングは流動的だと思われる。

ただ、3.x 特有の機能が増えていく中で、一石を投じる必要としては今がいいんじゃないかな、と思った次第である。
