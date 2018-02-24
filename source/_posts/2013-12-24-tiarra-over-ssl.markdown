---
layout: post
title: "Tiarra over SSL without stone"
date: 2013-12-24 00:08
comments: true
categories: [irc,tips]
---
[Tiarra](http://www.clovery.jp/tiarra/) といえば Perl で書かれた日本では著名な IRC Proxy であるが、[SSL 経由で接続しようと思ってぐぐる](https://www.google.co.jp/search?q=tiarra+ssl&amp;oq=tiarra+ssl) と 「[stone](http://www.gcd.org/sengoku/stone/Welcome.ja.html) 経由で接続する」 Howto がたくさん出てくる。果ては tiarra では SSL 接続できないから znc に移行した。とかいうエントリまで出てくる。

けれども、Tiarra のソースをちょこっと読んでみると綺麗にモジュール化されており、ソケット周りも綺麗にラップされてるので、簡単なソース変更でSSL接続ができるんじゃないか。というか、ここまでやる以上、作者である <a href="http://www.clovery.jp/">Topiaたん</a> がそれを意図して書かないわけがない。と思った。

そこで、[IO::Socket::SSL](http://search.cpan.org/~sullr/IO-Socket-SSL-1.962/) で [tiarra の現時点での最新ソース](http://coderepos.org/share/browser/lang/perl/tiarra) (rev.39276) を [ag(Silver Searcher)](https://github.com/ggreer/the_silver_searcher) すると、 main/Tiarra/Socket/Connect.pm の _check_connect_dependency 関数に以下のようにある。

```
sub _check_connect_dependency {
    my $this = shift;

    my $ssl = $this->current_ssl;
    if (defined $ssl && $ssl->{version}) {
        if (!Tiarra::OptionalModules->ssl) {
            $this->_warn(
                qq{You wants to connect with SSL, }.
                    qq{but SSL support is not enabled. }.
                        qq{Use non-SSL or install IO::Socket::SSL if possible.\n});
            return 0;
        }
    }
    return 1;
}
```

$ssl と $ssl->{version} が定義されていれば SSL 接続に行くらしい。
$ssl = $this-&gt;current_ssl なので、それを見ると

```
sub current_ssl {
    my $this = shift;

    utils->get_first_defined(
        $this->{connecting}->{ssl},
        $this->ssl);
}
```

てな感じで、 $this-&gt;{connecting} に ssl が定義されてると、それを返すらしい。
$this-&gt;connecting の中身は、下のようになっている。

```
$this-&gt;{connecting} = shift @{$this-&gt;{queue}};
```

$this-&gt;{queue} の中身は以下のようになっている。どうやら接続オプションを渡すようだ。

```
        foreach my $port (@ports) {
            push (@{$this->{queue}},
                  map {
                      $struct = {
                          type => $sock_type,
                          addr => $_,
                          port => $port,
                      };
                  } @{$addrs_by_types{$sock_type}});
        }
```

じゃあ多分接続オプションって tiarra.conf に書けるよね、と思ったら、[all.conf](http://coderepos.org/share/browser/lang/perl/tiarra/trunk/all.conf#L296) にあった。
これを参考にして ssl ブロック以下を接続に追記すると、あっさり stone なしで接続できた。
ただし、IO::Socket::SSL(と Net::SSLeay) が必要です。

verify オプションはデフォルト有効になっているので、証明書の検証に失敗して対処法がわからないようであれば no を指定してみると良いと思います。たとえば SSL が有効な freenode サーバに接続するなら、自分は今以下のように指定しています。(Debian GNU/Linux 7.3 wheezy の環境)

```
freenode {
  #  IP アドレスを指定すると名前解決が行われてエラーになる
  #  その場合は hosts の類に名前を書いてあげて、それを指定
  #  するようにする
  host: irc.freenode.net
  port: 7000

  ssl {
      version: sslv23
      ca-path: /etc/ssl/certs
  }
```

但し、host に IP アドレスを指定すると、 main/Tiarra/Resolver.pm で必要ない名前解決をしに行くためエラーになります(多分バグ)。そのため、host に IP アドレスを指定したくなった場合は hosts に類するものに適当な名前を書いてあげて、それを host の設定に指定すると繋がるようになります。

### まとめ

・ tiarra は 別に stone なしでも SSL 接続できる  
・ 要 IO::Socket::SSL(と Net::SSLeay)  
・ 必要な設定は最低限以下。 それ以外の設定は all.conf を参照。

```
ssl {
    version: sslv23

    # CA 鍵リストファイル。
    # verify (デフォルト有効)を行う場合は ca-file か ca-path の
    # どちらかを必ず指定してください。
    #ca-file: /usr/local/share/certs/ca-root-nss.crt

    # CA 鍵パス
    # verify (デフォルト有効)を行う場合は ca-file か ca-path の
    # どちらかを必ず指定してください。
    #ca-path: /etc/ssl/certs

    #  ca-path や ca-file を指定してもどうしても接続できない場合指定
    #verify: no
}
```

・[znc](http://znc.in/) 皆いいとか言ってるけど日本語の扱いイマイチだしその他細かいところも気に入らないしなんでC++なのか意味わかんないしで tiarra を維持できた自分はとても幸せです。  

・どうでもいいけど [CodeRepos](http://coderepos.org/) 落ちすぎな気がします。

**[ Update December 24th 23:34 JST by m ]**

Tiarra の作者の Topia たんから verify:no の件についていろいろ指摘を貰いました。 ca-path または ca-file で証明書のパスを指定しておかなかなったので verify に失敗していました。Debian GNU/Linux 7.3 wheezy の自分の環境では、ca-certificates パッケージと ssl-certs パッケージを入れて、 ca-path を指定すると freenode に verify した上で繋がるようになりました。

これを踏まえて、「まとめ」と freenode の接続例を更新しておきました。 <a href="http://www.clovery.jp/">Topiaたん</a> ありがとう！
