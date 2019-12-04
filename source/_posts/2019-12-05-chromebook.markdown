---
layout: post
title: "Chromebook"
date: 2019-12-05 03:53:10 +0900
comments: true
categories: [hardware]
---
[https://www.google.com/intl/ja_jp/chromebook/](https://www.google.com/intl/ja_jp/chromebook/)

最近、Chromebook に注目している。「Linuxクライアントを手軽に入手/維持したい」というニーズは以前から一貫して持っていたので、その手段の一つとして注目している。

特に自分の中で熱量が上がったのが、 [2019年発売の全てのChromebookがLinux対応にする](https://japan.zdnet.com/article/35136835/) と Google が表明したことだ。Chromebook はスペックが高いものでなければ3万円台から手に入ることから、自分のニーズを完全に満たせるのではないかと考えたのだ。

そこで、Linux に対応していることがわかっている [Acer R751-N14N](https://acerjapan.com/notebook/chromebook/spin11/R751T-N14N) を購入していろいろ試してみた。Linux 自体はまだベータ版なので、[設定から有効にする](https://support.google.com/chromebook/answer/9145439?hl=ja) 必要がある。

感想を並べると、以下のようになる。  
確かにお手軽ではあるものの、自分の中でまだまだだな、という印象だ。

- ハードのスペックが低いので、Linuxのコンテナ起動が重い  
- vimでファイル編集をしようと思ったら、ターミナルがおかしいのか、日本語が潰れる。自分だけなのかもしれないが、ChromeOS 組み込みのエディタを使ったほうが現状ファイル編集は早い  
- かといって、コンテナの中に X をまるごと入れようとは思わない   
- X Window System を入れて使い慣れたアプリを弄った方が自分は心地よい。Chromebook に Android アプリを入れて動かすのも悪くはないのだが、普段遣いとなると Chromebook は厳しい  
- リモートへの接続端末としてなら、悪くないかも
  
----
　  
最終的に、普段使っているLinux環境を再現したい欲が強いのだと思う。多分自分の中では、[GallumOS](https://galliumos.org/) を入れるか、リモートへの接続端末として使うのが最適解なのだろう...
