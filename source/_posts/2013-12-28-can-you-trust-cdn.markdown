---
layout: post
title: "Can you trust software distributed via CDN?"
date: 2013-12-28 23:19
comments: true
categories: [web]
---
皆様は CDN 経由で配布されるソフトウェアを信用しているだろうか。

最近は scriptタグ で埋め込む JavaScript のライブラリも増えてきて、CDN でそれを配布するプロジェクトも増えてきた。代表例が [jQuery](http://jquery.com/) である。jQuery は通常のjs や minify されたものの他に、以下のような CDN 経由での配布も可能にしている。

```
<script src="http://code.jquery.com/jquery-1.10.1.min.js"></script>
<script src="http://code.jquery.com/jquery-migrate-1.2.1.min.js"></script>
```

ダウンロードせず CDN に任せるということは、js のホストを外部に任せるということである。このことは以下の懸念に繋がる

a. インターネットに繋がっていないと開発できない   
b. CDN 上のファイルが動かない形に変更されたら困る   
c. CDN がサービスを停止したらどうするか   
d. プロジェクト自体が活動を停止し、配布をやめたときにソースが手に入らなくなる  

上記の理由から、自分は CDN を信用していない。jQuery の場合は、絶対に手元にダウンロードして利用する。自分の制御下にないところにホストを任せることほど危険なことはないと思う。

だが、自分の制御下にないソフトウェアを利用せざるを得ない場合がある。

JavaScript のグラフライブラリである [Google Chart](https://developers.google.com/chart/) はその代表例だ。これは [利用規約](https://developers.google.com/chart/terms) でライブラリをダウンロードすることを認めていない。バージョンアップと配布は Google にお任せである。こういうものに全面的に依存したサービスを作るのは、サービスの生死を Google に握られているようなものだ。現にこれを使っていて、ホスト先のファイルが勝手に互換性がない形で変更されて動かなくなり困ったことがある。

Google Chart を使ってから、自分の制御下にないソフトウェアはますます信用できなくなった。   
同僚は「開発段階では CDN を使ってもいいけど、Production にする段階で手元に引き上げてきてそれを使うかなー」と言っていたけど、自分的にはそれすら生ぬるいと思う。
