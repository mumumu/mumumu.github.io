---
layout: post
title: "Canon IP7230 は Ubuntu 16.04 では何もインストールしなくても動く"
date: 2018-02-26 05:40:13 +0900
comments: true
categories: [linux]
---
[http://cweb.canon.jp/drv-upd/ij-sfp/linux-pd-ip7230-380.html](http://cweb.canon.jp/drv-upd/ij-sfp/linux-pd-ip7230-380.html)

Linux のドライバが動く、という理由で Canon IP7230 を5年前だかに導入した記憶がある。当時は Ubuntu にもドライバが出ていたのだが、現在は... ? ということで試してみたら、ネットワークに繋いだら DNS-SD 経由で自動で発見してくれ、ドライバも CUPS が持っているエントリから見つけることができた。

いい時代になったものだ。

<img src="/images/canon-ip7200-ubuntu1604.png" width="540" height="960"/>
