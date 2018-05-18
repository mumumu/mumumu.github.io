---
layout: post
title: "Linux で Mac のようにスクリーンショットを撮る"
date: 2018-02-24 23:29
comments: true
categories: [linux, tips]
---

[https://github.com/naelstrof/maim](https://github.com/naelstrof/maim)

職場で Mac をデフォルトであてがわれることが多くなり、スクリーンショットを撮るのに Cmd+Shift+4 するのに慣れてしまった。そんな自分が、Linux で同じことをしたくなったときどうするか？

基本的には、以下のようになる。

A) 範囲指定でスクリーンショットを撮ることが出来るプログラムを探し、インストールする  
　 但し、GUI が呼ばれるプログラムは不可。CLI である必要がある  
B) 上記を呼び出すようにショートカットキーを割り当てる

だが、A) を満たすプログラムがなかなか見つからない。どれも GUI を呼ぶか、範囲指定ができてもborderがわからなかったりするなど使いにくいものばかりだった。 [Arch Linux のページ](https://wiki.archlinux.org/index.php/taking_a_screenshot) にあるソフトウェアをもろもろ調べ、結局選んだのが maim だった。

だが、こいつが Ubuntu だけ何故かパッケージが用意されていなかった(\*注1)ので、git リポジトリから最新版をインストールする羽目に。いろいろ依存していてビルドが大変だったが、Ubuntu 16.04 LTS では以下をインストールすれば(\*注2)、あとは [指示通りにすれば](https://github.com/naelstrof/maim#install-using-cmake-requires-cmake-git-libxrander-libxfixes-libglm) インストールできる。

```
$ sudo apt-get install  cmake libxrender-dev libxfixes-dev libglm-dev libglew-dev libglfw3-dev libgles2-mesa-dev libx11-dev libxcomposite-dev
```

あとは、以下のコマンドを Ctrl+Shift+F4 にマッピングした。  

```
$ maim -s ~/screenshot-$(date +%s).png
```

自分は KDE を使っているので [Command/URL を選んで Custom Shortcutを割り当てる](https://docs.kde.org/trunk5/en/kde-workspace/kcontrol/khotkeys/manage.html#manage-add-shortcut) 方法を使ったが、他のデスクトップ環境でも似たような機能はきっとあるだろう。

(\*注1) 他の主要ディストリビューションにはある (Debian 含) のに、Ubuntu だけ何故... (´ー｀; )  
(\*注2) 他に必要なものもあるかもしれない。build-essential とかは当然の前提よ(*゜ー゜)

**[Update 19th May 2018 05:39 JST by m]**  

Kubuntu 18.04 LTS では、ソースからビルドしなくても maim パッケージ一発で maim が入るようになっていました。しあわせ(｀ー´)
