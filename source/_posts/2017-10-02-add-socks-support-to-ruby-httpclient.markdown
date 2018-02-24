---
layout: post
title: "[PATCH] added socks support to Ruby httpclient"
date: 2017-10-02 04:27
comments: true
categories: [programming]
---
[https://github.com/nahi/httpclient/pull/380](https://github.com/nahi/httpclient/pull/380)

以下のような関係にある3つのホストがあり、server は firewall の外から名前解決もできないし、client からは ssh 経由でしか通信できないとする。また、firewall には curl(1) などのツール類はポリシー上一切入っていないとする。

```
+----------+          +-----------+         +------------+
|          |          |           |         |            |
|  client  +----------> firewall  +--------->   server   |
|          |   only   |           |         |            |
+----------+   ssh    +-----------+         +------------+
```

そこで、client から server の状態を http 経由でもろもろ確認したいとする。
以下のようにして、firewall に対して SOCKS プロキシを立てて通信したくなりはしないだろうか。

```
$ ssh -fN -D 9999 firewall
$ curl --socks5-hostname localhost:9999 http://server/
```

ただステータスコードを簡単に確認するためなら、これでも問題ない。だが、特定のプログラミング言語の HTTP通信ライブラリとかを使っていて細かい操作をしている場合、その通信ライブラリが SOCKS プロトコルに対応している必要がある。

自分の場合、上記に類似した状況 (*注1) で Ruby 製のHTTPクライアントライブラリを使っていて、やはり Ruby 側を対応したほうが後々のためにも便利だよな、と思い、 [httpclient の gem に対して patch を書いた](https://github.com/nahi/httpclient/pull/380)。これが取り込まれると、以下のようなことができるようになる。

```
require 'httpclient'

#  ssh -fN -D 9999 remote_server
clnt = HTTPClient.new('socks4://localhost:9999')

#clnt = HTTPClient.new('socks5://localhost:9999')
#clnt = HTTPClient.new('socks5://username:password@localhost:9999')

#ENV['SOCKS_PROXY'] = 'socks5://localhost:9999'
#clnt = HTTPClient.new

target = 'http://www.example.com/'
puts clnt.get(target).content

target = 'https://www.google.co.jp/'
puts clnt.get(target).content
```

つまり、HTTPプロキシと同じ感覚で SOCKS プロキシが設定できるようになる。
凄くニッチなニーズではあるけれども、取り込まれると良いな。

やりたいことがはっきりしていたので、Ruby の経験の多寡に関わらず、意外にすんなり書けた。また、自動テストのためには SOCKSサーバが必要だったことと、プロトコル自体は非常に簡単だったため、それも自前で実装した。 [Net::SSH::Proxy にクライアント実装があり](https://github.com/net-ssh/net-ssh/tree/master/lib/net/ssh/proxy)、それを拝借してきたお陰で、そんなに手間はかからなかった。

Ruby を使い始めてそんなに経っていないので、Ruby のコードを書くにあたってのお作法が殆どわからなかった。そこで、 [rubocop](https://github.com/bbatsov/rubocop) の助けを借りた。大したチェックはされないだろうとたかを括っていたが、コード上のメトリクスやベストプラクティスのあれこれまで細かく指摘されたのでびっくり。これを使ったお陰で、それなりに自信を持ってコードを書き進めることができた。

特定のプログラミング言語の初心者のうちは、可能であれば lint の類を使いながら書いてみると良いかもしれない。

(*注1) 実際はもっと複雑なのだが、そもそも、こんな状態を作っていること自体が筋が悪くて、firewall の中でなんとかすべき事案である。
