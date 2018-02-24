---
layout: post
title: "X-Forwarded-For"
date: 2013-06-13 22:49
comments: true
categories: [http]
---
[http://ja.wikipedia.org/wiki/X-Forwarded-For](http://ja.wikipedia.org/wiki/X-Forwarded-For)

少々大きなサイトになると、Webサーバ がロードバランサの後ろにあるなんて世界はもう当たり前になった。ロードバランサの後ろからは直接クライアントのIPアドレスを知ることが標準のやり方では取得しづらい。CGI でいうところの REMOTE_HOST は、ロードバランサのIPアドレスを意味するからだ。

それを取得するための方法が、X-Forwarded-For ヘッダの値なのは多くの人が知っている。けれども、これはデファクトではあっても標準ではない。今日たまたま商用のロードバランサを触っていて、このヘッダの値を設定してこないのに驚いた。

何らかの値は設定するモノだよなーと思ってCGIに渡される環境変数やHTTPヘッダの値をぱっと取得する方法をちょびっと考えたら、PHP が一番楽だなーと個人的には思いました。みんな馬鹿にするけどね。

	<?php
	var_dump($_SERVER);

だけでいいんだもの。  

Python を CGI として実行させるんだったら以下のようになる。  
CGI として実行するんで、拡張子や実行権限とかを気にしないといけないのはちょっとアレ。

	#!/usr/bin/env python 
	import cgi
	cgi.print_environ();

Ruby だったら...？

	#!/usr/bin/env ruby
	print "Content-type: text/html\n\n"
	ENV.collect() do |key, value|
    		print key + " --> " + value + "<br />"
	end

Perl だったら...？ というのは省略。  

たぶん、Python や Ruby も Perl も(フレームワークとかでキックされる状況等という意味で)実行環境によってはもっと短い書き方があるはずだ。結局、自分が一番使い慣れた LL が PHP ってだけなんだと思う。
