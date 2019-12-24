---
layout: post
title: "Chrome のタブやアドレスバーが小さい時"
date: 2019-12-24 21:14:20 +0900
comments: true
categories: [misc,tips]
---
[https://superuser.com/questions/982560/chrome-tab-font-too-small/982565](https://superuser.com/questions/982560/chrome-tab-font-too-small/982565)

あれはいつだったからか。Linux で Google Chrome のアドレスバーやタブのフォントが小さすぎる問題にずっと悩んでいた。

最初は 2560 x 1920 のディスプレイで Chrome を表示させた時だったと思う。別の時は、NVDIA なビデオカード上で Linux を動かしたときとか。とにかく、自分に丁度いい塩梅で Google Chrome のアドレスバーやタブのフォント の大きさが表示できたことが最近までなかった。

最近 ThinkPad X395 に Linux を入れて使い始めたのだけど、同じ状態になった。  
いい加減何とかならんかなと思ってぐぐりまくると、上記の記事に行き着いた。

これは Windows の記事だが、なんと X Window System 上でも通用したのでびっくりした。  
もしあなたが自分と同じ問題に悩んでいるのなら、コマンドラインから、以下のようにして Google Chrome を起動してみるとよい。

1.5 の数値は、自分の環境に合わせて適度に調整可能である。

```
$ google-chrome --force-device-scale-factor=1.5
```

今は自分でこれらのサイズを調整できるようになって、とてもしあわせだ(｀ー´)
