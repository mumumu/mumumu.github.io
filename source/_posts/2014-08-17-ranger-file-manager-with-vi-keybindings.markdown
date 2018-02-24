---
layout: post
title: "ranger - file manager with VI keybindings"
date: 2014-08-17 00:58
comments: true
categories: [software]
---
[http://ranger.nongnu.org/](http://ranger.nongnu.org/)

比較的大きな規模のプロジェクトになると、ディレクトリツリーの階層もそれなりに深くなる場合が多い。しかも同じディレクトリに常にとどまって作業をしていればいいという話にならなくなってくる。ディレクトリを移動する度に cd /path/to/foo とか cd /another/path/fuga とかもう打ってられないことに気がついた。

vim とかのエディタ内のファイルマネージャーを使えばいいじゃない、と一時期は考えて [netrw](http://www.vim.org/scripts/script.php?script_id=1075) とか [nerdtree](https://github.com/scrooloose/nerdtree) とかを我慢して使っていた時期もあったが、どうもディレクトリの移動の度に[リターンキーを「ッターン！」](https://www.google.co.jp/search?q=%E3%83%83%E3%82%BF%E3%83%BC%E3%83%B3%EF%BC%81) しなければいけないのがつらく、結局使わなくなった。

ある日、コンソール上で画像ファイルをプレビューしたくなった。そんなソフトはないかと捜していたら、[ranger](http://ranger.nongnu.org/) にいきついた。いざ使ってみると、ファイルやディレクトリの移動、そしてファイルを開く操作が全て hjkl だけで済むのが異常に心地よいことに気がついた。[w3m](http://w3m.sourceforge.net/) を入れて設定に一行足せば、以下のように画像ファイルのプレビューも出来る。cd /path/to/foo/bar するよりはかなり速い。 また、ranger を開きながら Shift+s でシェルに落ちることもできる。これもかなり楽。

<img src="/images/ranger.png"/>

また、vim の場合だと、ファイルを開きながら、別のファイルを選んで開くときも、 netrw とかを使わずに ranger を使えるモードがあることに気がついた。[こんな設定](https://github.com/hut/ranger/blob/master/doc/examples/vim_file_chooser.vim) を ~/.vimrc に書けば、&lt;leader&gt;r を押すことで ranger でファイルを選ぶことができる。

そんな感じで喜んで使っていたのだが、Pythonのファイルを扱っていると若干問題があった。types.py というファイルがディレクトリに存在すると、それを vim から ranger で開こうとした時点でエラーになってしまうのだ。

```
Traceback (most recent call last):
  File "C:\Python27\lib\site.py", line 62, in <module>
    import os
  File "C:\Python27\lib\os.py", line 398, in <module>
    import UserDict
  File "C:\Python27\lib\UserDict.py", line 83, in <module>
    import _abcoll
  File "C:\Python27\lib\_abcoll.py", line 70, in <module>
    Iterable.register(str)
  File "C:\Python27\lib\abc.py", line 107, in register
    if not isinstance(subclass, (type, types.ClassType)):
AttributeError: 'module' object has no attribute 'ClassType'
```

これは ranger 自体が Python で書かれていることと、[Pythonインタプリタ自体の問題](http://stackoverflow.com/questions/17717090/attributeerror-module-object-has-no-attribute-classtype) が重なったことによって起こることだった。けれども、今の職場で主に使われているのは Python なので、どうにかして解消しなければ ranger は使えないという話になってしまう。

仕方なく、[細かすぎて伝わらないpatch](https://github.com/mumumu/ranger/commit/d4791468897ab4136edceabe640360110dfd810e) を書いた... vim ファイルだけではなくて、コアのイベントアクションにまで手を加えているので、全く受け入れられる気がしない... それに vim 連携のためだけに types.py を特別扱いするとかアフォかと。俺がメンテナなら多分 Reject するだろうなと思いつつ、fork したコードだけは置いておくのであった(´ー｀; )

[https://github.com/mumumu/ranger](https://github.com/mumumu/ranger)
