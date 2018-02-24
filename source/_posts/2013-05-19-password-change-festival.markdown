---
layout: post
title: "Password Change Festival"
date: 2013-05-19 23:19
comments: true
categories: [security]
---
[http://keepass.info/](http://keepass.info/)  
[ヤフー、最大2200万件のIDが流出の可能性--確認ツールも公開](http://japan.cnet.com/news/business/35032224/)  

Yahoo Japan ID が2200万件も流出したとの報を受けた。直ちに認証情報流出に繋がるとは限らないが、これを機にパスワード管理もきちんとせなあかんな、ということでいろいろなサイトのパスワードを変更してまわった。

パスワード管理には KeePass Password Safe を使う。これは Windows Only だと最初思っていたのだが、よく調べてみると [mono](http://www.mono-project.com/) が動く環境であれば動くとのこと。よく見てみると、Debian 系でも 以下のコマンドで一発インストールが可能だった。自分の環境は [Kubuntu](http://www.kubuntu.org/) なので、楽ちん楽ちん。

	sudo apt-get install keepass2

いくつかのサイトはパスワードが同じものがあった。強度が高いパスワードを生成するようにしてそれらを全て置き換えた。あと、Yahoo Japan の ID は実質未使用だったので全て削除した。数はできるだけ減らすに限る。

管理対象のサービスやサーバを数えてみたら、合計24あった。まあ、そんなものか。


