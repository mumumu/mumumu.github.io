---
layout: post
title: "Rotate Image depending on exif:orientation value"
date: 2013-05-15 00:26
comments: true
categories: [java,image,iphone]
---
[http://beradrian.wordpress.com/2008/11/14/rotate-exif-images/](http://beradrian.wordpress.com/2008/11/14/rotate-exif-images/)

スマートフォンで写真を撮り、それをクラウドに保存するということはよくある。だが、意図的にではないにせよ、その写真の天地や縦横が逆になったりすることがある。[これは特に「+」のボリュームボタンでシャッターが切れるようになった iPhone5 から顕著だ](http://iphone3gsman.blog71.fc2.com/blog-entry-424.html)。

画像自体がどのような方向で保存されているのかは、[EXIF(Exchangeable Image File Format)](http://www.exif.org/) に保存されているのが普通だ。最近の画像処理ライブラリは、こういうことも EXIF:Orientation の値を見て手当てするようになっている。

自分もたまたまそういう処理を Java で書いていて、「iPhone で写真を撮ると写真が逆さまにアップロードされる」というトラブルに出会った。[jMagick](http://www.jmagick.org/) を使って画像をいったんある一定の大きさに変換してファイルに保存していたのだが、このプログラムは古かったため EXIF を見ることはしていなかったのが原因だった。画像の方向とEXIF の関係は一番はじめに示したリンクに詳しいが、jMagick の場合必要な処理を自前でやらなければならない。

[一番はじめに示したリンク](http://beradrian.wordpress.com/2008/11/14/rotate-exif-images/)  はまさに同じケースを扱っており大いに参考になった。しかし、あいにくとここに載っているコードには以下の問題があった。

a) 複数回同じ画像で変換が走った場合のことを全く考慮していない。

EXIF の値を見て変換するのはよいのだが、いったん変換した後にまた同じ回転や反転処理が走ってしまうと元に戻ってしまったりする。これを防ぐためには、EXIF の値を更新し、２度同じ処理が起こらないようにする必要がある。

b) jMagick のバックエンドである [ImageMagick](http://www.imagemagick.org) は EXIF の更新に対応していない

これも残念ながら自前で書かねばならない。EXIF を更新するためのライブラリは様々なものがあるので好きに使うと良い。自分は apache-sanselan (現在は [commons Imaging](http://commons.apache.org/proper/commons-imaging/)) を使った。

- - - -
<br />
これらを踏まえて、[一番はじめに示したリンク](http://beradrian.wordpress.com/2008/11/14/rotate-exif-images/) に示されているコードを書き直したの以下である。EXIF の更新コードを Java で書く場合って、Web を調べても割と中途半端にしか載っていないことが多かった。その意味では、完全なコードを書き残しておくことは意味があるだろう。

<script src="https://gist.github.com/mumumu/5576751.js"></script>
