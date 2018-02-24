---
layout: post
title: "Silly patch - Octopress does not open new entry file immediately"
date: 2014-08-30 20:50
comments: true
categories: ['patch']
---
github Page を github で管理するためのソフトウェア [Octopress](http://octopress.org/) では、新しいエントリを書くときにいちいちエディタを開くコマンドを打たねばならない。最初は気にならなかったのだが、[ranger を導入したあたり](http://mumumu.github.io/blog/2014/08/17/ranger-file-manager-with-vi-keybindings/) からそれがどうにも煩雑だと感じるようになった。

こんなの誰でも思いつくだろと思って Octopress の github を調べると、[こんな Pull Request](https://github.com/imathis/octopress/pull/749) が見つかり...

これをよく読んでいくと以下のように、「俺たちの流儀ぢゃねえからこれに類する提案は皆 Reject だよハゲ」という話になっていて...そもそもこういう路線の patch は本体に取り込まれる見込みがないことが明確になっていた(´ー｀; )

[https://github.com/imathis/octopress/pull/749#issuecomment-11451776](https://github.com/imathis/octopress/pull/749#issuecomment-11451776)

なので、仕方なく自分は必要な部分のみ糞 patch を当てるわけです。。  

[https://github.com/mumumu/mumumu.github.io/commit/a139bf79c3af9346c1f607ef04acce6e442a32df](https://github.com/mumumu/mumumu.github.io/commit/a139bf79c3af9346c1f607ef04acce6e442a32df)

またもや下らぬ patch を書いてしまった。。これもうシリーズ化しようかなって思うんですけど、どないですかね(´ー｀; )
