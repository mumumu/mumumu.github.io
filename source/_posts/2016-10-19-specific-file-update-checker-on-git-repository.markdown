---
layout: post
title: "Specific file update checker on Git Repository"
date: 2016-10-19 06:26
comments: true
categories: [programming]
---
[https://github.com/mumumu/repos_file_update_checker](https://github.com/mumumu/repos_file_update_checker)

以前 PEP 8 のファイル更新だけをチェックしたくて、 [Mercurial 向けのファイルチェッカを書いた](http://mumumu.github.io/blog/2015/12/26/silly-patch-could-not-detect-repository-update/) 。  
今年になって [Python のリポジトリが Github に移行した](https://github.com/python/) ため、それが役に立たなくなった。よって、以前書いたものを Git にも対応させ、プラグイン化してかっこ良くしたものが上記である。

みんな大好き Github ... といえども特定ファイルの git log 出力を RSS で吐いてくれたりはしないんですよね。。
