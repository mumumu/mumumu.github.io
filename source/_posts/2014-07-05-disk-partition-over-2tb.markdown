---
layout: post
title: "disk partition over 2TB"
date: 2014-07-05 22:22
comments: true
categories: [linux]
---
ストレージって普段意識せずに使うモノだから、ハードウェア障害とかでいざ交換とか再初期化とかいう話になると、オペレーションを思い出すのに一瞬うっとなることが多い。今日もそんな日だった。

ちょっと重要なサーバのストレージが死にかけたので、代替機を作ろうという話になった。ストレージは 480GB のSSD と Western Digital の 3TB。/dev/sda, /dev/sdb に割り当てられているのはわかっている。どちらがSSDで、どちらがHDDか？

今までは dmesg とかを目grepしてどちらが何であるかを見ていた。なんとも原始的で情けない話だ。  
けれども、hdparm 使えば簡単だということを教わった。

```
nn@op3:~$ sudo hdparm -i /dev/sda

/dev/sda:

 Model=WDC WD30EZRX-00AZ6B0, FwRev=80.00A80, SerialNo=WD-WCC070146118
 Config={ HardSect NotMFM HdSw>15uSec SpinMotCtl Fixed DTR>5Mbs FmtGapReq }
 RawCHS=16383/16/63, TrkSize=0, SectSize=0, ECCbytes=50
 BuffType=unknown, BuffSize=unknown, MaxMultSect=16, MultSect=16
 CurCHS=16383/16/63, CurSects=16514064, LBA=yes, LBAsects=5860533168
 IORDY=on/off, tPIO={min:120,w/IORDY:120}, tDMA={min:120,rec:120}
 PIO modes:  pio0 pio3 pio4 
 DMA modes:  mdma0 mdma1 mdma2 
 UDMA modes: udma0 udma1 udma2 udma3 udma4 udma5 *udma6 
 AdvancedPM=no WriteCache=enabled
 Drive conforms to: Unspecified:  ATA/ATAPI-1,2,3,4,5,6,7

 * signifies the current active mode
```

今回は /dev/sda に何故か HDD が割り当てられていることがわかった。パーティーションを一つだけ切って、それに全ての容量を割り当て、ext4 でフォーマットしようぜ、という話になった。自分は fdisk でこれまでパーティーションを切ることに慣れていたので、fdiskでパーティーションを切ったところ、以下のように2TB分しか切られない。なんでだ。

```
/dev/sda1                2.0T  199M  1.9T   1% /hoge
``` 

と思ったら、Linux の fdisk は [GUIDパーティーションテーブル(GPT)](http://ja.wikipedia.org/wiki/GUID%E3%83%91%E3%83%BC%E3%83%86%E3%82%A3%E3%82%B7%E3%83%A7%E3%83%B3%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB) に対応していないので、 2TiB 以上のパーティーションは作れないようだ。

```
$ sudo parted /dev/sda
(parted) mklabel gpt
(parted) mkpart primary 0% 100%
(parted) print
Model: ATA WDC WD30EZRX-00A (scsi)
Disk /dev/sda: 3001GB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt

Number  Start   End     Size    File system  Name     Flags
1      1049kB  3001GB  3001GB               primary
```

これで 2TB 以上のディスクを普通に扱えるようになった。これまでも2TB以上のディスクに普通に fdisk を使ってきた自分が恥ずかしいわ(´ー｀；)
