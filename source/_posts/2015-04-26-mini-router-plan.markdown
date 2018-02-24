---
layout: post
title: "mini router plan"
date: 2015-04-26 03:48
comments: true
categories: [hardware]
---
[http://blog.livedoor.jp/connect_staff/archives/1804038.html](http://blog.livedoor.jp/connect_staff/archives/1804038.html)

自分が使っているプロバイダが突如サービス終了するというアナウンスがあった。[代替のプロバイダはもうほぼ決めた](http://www.din.or.jp/) のだけど、これを機会にブロードバンドルータもどうにかしようという気になった。[RTX1210](http://jp.yamaha.com/products/network/routers/rtx1210/) を中古で買うか、マシンをもう一台組んで [VyOS](http://vyos.net/wiki/Main_Page) を入れてしまう案の二つが上がったが、結果として後者、つまりマシンを新たにもう一台組むことにした。

・ ルータに使うマシンなので、大きなマシンは組みたくない  
　・ Mini-ITX がいいんじゃないですかね  
・ ファンレスがいいよね。じゃあ前欲しがってた Atom C2000 シリーズあたりでいきますか

という思考から、以下のような構成を考えたのでメモしておく。メモリを16GBにする必要がどこにあるのかというツッコミは禁止します。

Mini-ITX はそのコンパクトさ故に電源やケースの組み合わせに注意が必要で、特にケースと電源選びは非常に面倒であった。電源は SFX電源とか考えるのが面倒だったので ATX 電源、かつ奥行き140mm で300W 以下、となると玄人指向のものしか残らなかった。Mini-ITX のケースは自分が考える最小のコンパクトさと電源のサイズをベースに考えていった結果、バランスのとれたモノとして Silverstone のものを選んだということである。

85k は安くはない。RTX1210 はこれより中古で安く買えるのでかつては迷ったが、やはり手元の自由度を選ぶと自作が最高だ。いつ組もうかなぁ(´ー｀；)

パーツ項目 | 品名 | 価格
---------- | ---- | -------
CPU        |  Intel Atom processor C2750, SoC, FCBGA 1283, 20W 8-Core | -
マザーボード | [Supermicro A1SAi-2750F](http://www.supermicro.com/products/motherboard/atom/x10/a1sai-2750f.cfm) | 54,647
RAM        |  [SanMax Technologies SMD-N16G28ECTP-16KL-D](https://www.ark-pc.co.jp/i/11702324/) | 17,990
ケース      | [Silverstone SST-SG13B](https://www.ark-pc.co.jp/i/15300667/) | 7,538
電源       | [玄人志向 80PLUS BRONZE取得 ATX電源 300W KRPW-PB300W/85+](http://www.kuroutoshikou.com/product/power/atx/krpw-pb300w_85_/)  | 5,674
合計       |                                  | 85,849
