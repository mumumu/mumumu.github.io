---
layout: post
title: "Debian GNU/Linux 7.0 Wheezy"
date: 2013-05-05 23:21
comments: true
categories: [linux,debian]
---

[http://www.debian.org/News/2013/20130504](http://www.debian.org/News/2013/20130504)  
[http://www.debian.org/releases/wheezy/releasenotes](http://www.debian.org/releases/wheezy/releasenotes])

今のところ手元で運用している全仮想マシンが Debian なので、早速アップデートした。アップグレードに関する手順や注意点はリリースノートに全て纏まっていることと、Fedora とかと違って Debian のアップグレードは凄く安定しているのでそんなに手間を掛けなくても移行できる。

バックアップや細かいチェックを除けば、/etc/apt/sources.list の squeeze を wheezy に書き換えて以下を実行するのみだ。

	$ sudo apt-get update
	$ sudo apt-get upgrade
	$ sudo apt-get dist-upgrade

自分が [nagios](http://www.nagios.org/) をソースから入れていたことから、[insserv](http://wiki.debian.org/LSBInitScripts/DependencyBasedBoot) まわりで衝突が起きて止まったことと、 [Ruby を 2.0.0-p0 にしていて](/blog/2013/04/27/ruby-2-dot-0-0-p0/) sudo -u mumumu ruby .... と rc.local に書いていた部分が動かなくなったことを除けば順調に進行。 楽チンだなー。この移行の安定性が サーバに Debian を使う一番の理由だったりする。

![debian upgrade complete!](/images/debian_wheezy_upgrade_complete.png)
