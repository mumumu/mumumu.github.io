---
layout: post
title: "Disable Akonadi"
date: 2014-05-12 11:25
comments: true
categories: [kde]
---
[http://userbase.kde.org/Akonadi#Disabling\_the\_Akonadi\_subsystem](http://userbase.kde.org/Akonadi#Disabling_the_Akonadi_subsystem)

Kubuntu 14.04 LTS には KDE 4.13 が入っているが、このバージョンから有効になっているデスクトップ検索がどうにも糞で、起動して10分くらいCPUを毎回占有してくれる。

業を煮やして無効にした。以下のコマンドを実行 (rootである必要はない) して、上のリンクにあるとおり、カレンダーのイベント表示を無効にすればよい。実はこのやり方は Akonadi サブシステムを無効にするものだが、Akonadi を使っている機能はどれも俺は使ってないので一気に無効にしたった(´ー｀; )

```
$ akonadictl stop
```

カレンダーのイベント表示は以下のように "Display events" となっているので、そのチェックを外す。

<img src="/images/disable_akonadi_on_calendar.png" />
