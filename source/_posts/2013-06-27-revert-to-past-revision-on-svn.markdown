---
layout: post
title: "revert to past revision on svn"
date: 2013-06-27 06:58
comments: true
categories: [subversion,howto]
---
自分がやっているタスクでは、たまーーーに別の言語のツリーにコミットされるというアフォな現象が起きる(注1)。その場合、過去のリビジョンに遡って元に戻したい場合がある。Subversion でその手順を良く忘れるのでメモ。

	$ svn merge -r 330684:330682  .
	--- r330684 から r330683 までを '.' に逆マージしています:
	U    configure.xml
	U    functions/opcache-invalidate.xml
	U    functions/opcache-reset.xml
	D    entities.functions.xml
	U    book.xml
	
要するに過去の誤った変更をリバースパッチとしてマージしている。その中身を確かめた後、コミットすると良い。

	$ svn ci -m 'reverted r330683, r330684 changes because of ja tree broken sorry.'
	送信しています              opcache/book.xml
	送信しています              opcache/configure.xml
	削除しています              opcache/entities.functions.xml
	送信しています              opcache/functions/opcache-invalidate.xml
	送信しています              opcache/functions/opcache-reset.xml
	ファイルのデータを送信しています ....
	リビジョン 330685 をコミットしました。

(注1) これは、他の言語のツリーからディレクトリを丸ごとコピーした後、.svn ディレクトリを削除していなかったことが原因だった。。.svn ディレクトリの中身は他の言語の内容を見ているので、コミットもそっちに行ったというわけだ。。[高木神](http://d.hatena.ne.jp/takagimasahiro/)さんくすです ... orz
