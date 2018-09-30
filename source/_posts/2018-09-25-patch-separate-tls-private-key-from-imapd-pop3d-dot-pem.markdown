---
layout: post
title: "[PATCH] Courier の TLS 証明書と秘密鍵ファイルを分離する"
date: 2018-09-25 20:36:39 +0900
comments: true
categories: [patch]
---

このエントリは、以下の Pull Request がマージされるまでの記録である。  
[https://github.com/svarshavchik/courier-libs/pull/13](https://github.com/svarshavchik/courier-libs/pull/13)

### 3行まとめ

1) [Courier Mail Server](http://www.courier-mta.org/) (以下 Courier) という古いメールサーバに patch を書いてマージしてもらった  
2) いろいろ苦労しながら、やり遂げた記録  
3) 青臭いソフトウェアエンジニアも、成長するものだと思う

### なぜこの patch を書いたか

始まりは、[Let's Encrypt](https://letsencrypt.org/) を手元のサーバに導入しようとして、 [Courier IMAP](http://www.courier-mta.org/imap/) にも適用しようとしたことだった。だが、一筋縄では行かなかった。どうしても TLS の通信に失敗するのだ。いろいろググった結果、Courier では、[「TLS の秘密鍵と証明書は同一のファイルに入っていなければいけない」ことがわかった](https://yeri.be/postfix-courier-letsencrypt)。

多くのサーバソフトウェアでは、TLS の秘密鍵と証明書のファイルパスは別々に設定できる。メールサーバで言えば [Postfix でもそうだ](http://www.postfix.org/postconf.5.html#smtp_tls_key_file)。このことから、自分にとっては Courier のこの動きは直感に反する仕様であった。しかもこれに関するドキュメントが **一切書かれていない** ことにカッとなった。

だから、[issueを立てて、秘密鍵と証明書を分離することを提案した](https://github.com/svarshavchik/courier/issues/10) ところ、メンテナ的にはデフォルトの動きに fallback 出来るなら問題ないよ、とのことだったので、[patch](https://github.com/svarshavchik/courier-libs/pull/13) を書いた。このエントリは、それがマージされたことの記録である。

### 当初はヤクの毛刈りに奔走

patch を書くのは一筋縄では行かなかった。当初はヤクの毛刈りに奔走させられた。当初は git のソースをまともにビルドすらできなかったので、ビルドをキックするスクリプトを修正することから始まり、メンテナと自分の開発するディストリビューションが異なる (\*1) ことが原因の libtool まわりの修正から、ドキュメントの修正まで、送った patch は 6つに及んだ。

メンテナにしてみれば、自分以外の環境で patch を書く人がいるとは思ってもいなかったのだろう。  

a) [Courier 本体側で 送った patchたち](https://github.com/svarshavchik/courier/pulls?q=is%3Apr+is%3Aclosed+author%3Amumumu)  
b) [Courier Library 側で送った patch たち](https://github.com/svarshavchik/courier-libs/pulls?q=is%3Apr+is%3Aclosed+author%3Amumumu)  

(\*1) メンテナは Fedora で、自分は Ubuntu だった。

### OpenSSL + GnuTLS の両方に対応

一通りコードを書く環境が整い、途中まで patch を書いてみると、Courier は TLS 通信に OpenSSL だけでなく、 [GnuTLS](https://www.gnutls.org/) も並行してサポートしていることに気がついた。また、[SNI(Server Name Indication)](https://tools.ietf.org/rfc/rfc6066.txt) や IPアドレスベースのバーチャルホストもサポートしていたので、それらもこれら両方のライブラリでサポートするように書かなければならなかった。

さらに、これら2つのライブラリのコードは、実装されている機能がそれぞれに微妙に異なっていたので、どこまで実装するかを判断するために全コードを見る必要があり、対応は難航した。方針は以下に纏めた通りだが、正直 GnuTLS 側のコードは、よく [読めた|書けた] ものだと今振り返ってみて思う。

c) [couriertls の TLS_PRIVATE_KEY 対応方針](https://gist.github.com/mumumu/650461670436c52a74b06e04725f78ab)

### モチベーション

何故ここまで続いたのかは、正直わからない。本職で C++ を書くようになり、この手のソフトウェアの深追いには慣れていたことが割と大きかったように思う。あとは、少しずつでも、ゴールに近づいている感覚が確実にあったからだろうか。また、義務感なしに、気が向いた時に作業できたのも大きい。

### メンテナについて

Courier のメンテナは、 [Sam Varshavchik](https://www.linkedin.com/in/cplusplusguru) というロシア人だ。コードが良ければ黙ってマージしてくれるし、提案にもちゃんと真摯に返事を返してくれる人だった。コード自体は正直綺麗だとは思わないが、OSS ではコードを全面的に書き直すのでもない限り、相手の流儀に徹底的に合わせるのが基本なので、メンテナの流儀を踏襲する方向で動いた。

### その他

Courier 自体は 1990年代末からある古いソフトウェアである。自分は 自宅サーバを立て始めたのが2004年で、その時は他の人が書いた手順を見よう見まねで自分の環境にいろいろ適用しては試行錯誤を繰り返していた。 [スラッシュドットの日記](https://srad.jp/~mumumu/journal/184844/) にも、当時の青臭い記録が残っている。そんな糞エンジニアな自分が、こうして、patch を書くことなど思いもよらなかった。

当時から、少しは成長したんだなあ、と思う。
