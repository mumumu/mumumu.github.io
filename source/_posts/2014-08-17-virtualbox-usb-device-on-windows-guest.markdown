---
layout: post
title: "Virtualbox USB device on Windows guest"
date: 2014-08-17 00:55
comments: true
categories: [memo]
---
[http://www.igune.com/20140529/1637-40/](http://www.igune.com/20140529/1637-40/)

VirtualBox で Windows ゲストを動かしていると毎回忘れるのが、vboxusers グループに自分のユーザを追加することである。これがないと Windowsゲストで USB機器が認識されない。

けど、apt パッケージとかでアップデートの管理をしていると、アップデートの度にグループ設定が消えてしまうことがあった。仕方なく以下を .bashrc に追加したが、いかにもダサい... virtualbox がインストール済みなのが前提だし(´ー｀; )

```
VBOXGROUP="vboxusers"
id | grep $VBOXGROUP > /dev/null 2>&1
if [ $? -ne 0 ]; then
    echo "$USER added to $VBOXGROUP group"
    usermod -G $VBOXGROUP $USER
fi
```

はじめに示したリンクは Windows7 で説明されているが、それに限った話ではない。Windows 8.1 でも起こる話だ。
