---
layout: post
title: "Fabric Essentials"
date: 2013-11-14 12:36
comments: true
categories: python
---
<iframe src="http://www.slideshare.net/slideshow/embed_code/28142569" width="427" height="356" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen> </iframe> 

[自宅のサーバで遊びで使っていた](http://mumumu.github.io/blog/2013/05/09/fabric/) [Fabric](http://fabfile.org) をいつの間にか業務で使うことになってしまった。[Capistrano](https://github.com/capistrano/capistrano) でもよかったのかもしれないが、Python で書けることで皆がインフラ周りのオペレーションに違和感なく手を染められるように、という目的が一番大きかったと思う。Ruby なソフトウェアはそれと心中するような勢いでないと長期的な運用は厳しい、という認識もあったと思う。

使い込んだら使い込んだでそれなりにノウハウはあった。自分でタスクを書いたり、他の人を誘導している間にたまったノウハウを、社内でLTという形で喋ったのが上のプレゼンテーションだ。特に env.disable_known_hosts = True としないと [paramiko](http://www.lag.net/paramiko/) がガチで遅くなったことには閉口した。数台で遊ぶレベルとの違いを感じるのがやはり大規模サイトの面白いところだ。あとはこのスライドでも再三強調している通り、ホストの指定は注意を払わないとおかしな挙動にはまりやすいのが中心的なノウハウと言えるかもしれない。

レベルとしては初心者向けにあたるこのスライドに[なんで反響があるのか](http://b.hatena.ne.jp/entry/www.slideshare.net/mumumu/fabric-essentials-28142569) 驚かなくもないが、需要があるということなのだろう。
  
  
また、このスライドではちらっと自分の所属を公開している。自分の中では所属をインターネットで公開しないことは長年守ってきたポリシーのひとつである。それを転換したことが、現在の所属に満足しており、自信を持つことができていることの現れだと思う。まだまだ至らないところが多い自分を大人のエンジニアとして扱ってくれ、様々なことにチャレンジさせてくれるチームには深く感謝している。

そして、喋ることにも大分慣れてきた。自分の知見を開陳することにはためらいがないこともなかったが、それも最近消えつつある。様々なところからフィードバックを貰えることも楽しめているので、アウトプットする手段の一つとしてやっていきたい。
