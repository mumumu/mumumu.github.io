---
layout: post
title: "phpdoc_rev_compare.py"
date: 2013-06-27 04:31
comments: true
categories: [php,python]
---

PHP のドキュメントは毎日凄い勢いで更新されている。これについていくのは [高木神](http://d.hatena.ne.jp/takagimasahiro/) も面倒だと思っているはずである。だが、[revcheck.php](http://doc.php.net/php/ja/revcheck.php?p=missfiles) をいちいち目視で確認するなんて俺の中では最悪だ。これがどこまで機能してるかだってわからない。

けど、最近 [php-doc-ja ML](http://ml.php.gr.jp/mailman/listinfo/php-doc) に来たちょろちょろとした報告を修正だけしているやる気のなさっぷりにも嫌気がさしてきた。やっぱり日々追加されるドキュメントだけでも(たとえそれが未翻訳であっても)追いついておくべきだ。と思った。

それを自分の満足行く形で抽出し、反映するために以下のスクリプトを書いた。最終更新リビジョンだけ見ることが出来れば、少なくともおおまかな差分情報だけは判別できる。これを足がかりにすれば、「片方に全く反映されていないモノ」については自動同期が可能なはずではある。いきなりやると怖いのでやってないけれども。

<script src="https://gist.github.com/mumumu/5870407.js"></script>
