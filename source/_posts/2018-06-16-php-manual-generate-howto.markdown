---
layout: post
title: "PHP manual generate HOWTO (version 2018-06-16)"
date: 2018-06-16 20:18
comments: true
categories: [php,howto]
---
[http://mumumu.github.io/blog/2013/06/29/php-manual-generate-howto/](http://mumumu.github.io/blog/2013/06/29/php-manual-generate-howto/)

このエントリは、5年前に書かれた上記エントリの更新版である。  
PHP のバージョンを 5.x 系から 7.x 系にあげたのだが、やり方は何も変わっていない。ただ、PhD のバグが fix されているので、chm のビルド手順が単純になっている。

特に更新する必要はないかとも考えたが、最新バージョンでの手順を示すのも意義があると考え、脚注を少し足した上で、このエントリを記す。

----
<br />
PHPのマニュアルは、DocBook と呼ばれるフォーマットで記述されており、[PhD という PHPマニュアル のためのツール](https://wiki.php.net/doc/phd) によってその DocBook から HTML や CHM、PDF などの複数の形式にビルドできる。

この記事では、2018年6月時点で実際にPHPのマニュアルをリポジトリから取ってきてビルドする方法を紹介する。PhD は PDF や manpage形式、epub 等多様な形式がビルドできるが、ここでは HTML および chm のみについて触れる。

### a) ビルドするときに使うPHPについて

まず、PHP をインストールしなければはじまらない。  
2018年6月現在、PHP 7.2.6 が最新版である。このバージョンで問題なく HTML 版も chm もビルドできる。

PHP をインストールするときも、PhD のために DOM, libXML2, XMLReader and SQLite3 は有効にしていなければならない。ただ、これらは デフォルトで有効になっているので、無効にしないよう注意しよう。

### b) 必要なツールをインストール

subversion と PEAR を予めインストール(\*1)しておいて、PhD と PhD_PHP をpearコマンド経由でインストール(\*2)する。  
PhD_PHP は chm をビルドするのに必要だ。

	$ pear version
	PEAR Version: 1.10.5
	PHP Version: 7.2.6
	Zend Engine Version: 3.2.0
    Running on: Linux 4.15.0-23-generic #25-Ubuntu SMP Wed May 23 18:02:16 UTC 2018 x86_64
	$ pear install doc.php.net/PhD doc.php.net/PhD_PHP
	WARNING: channel "doc.php.net" has updated its protocols, use "pear channel-update doc.php.net" to update
	Did not download optional dependencies: phpdocs/PhD_PEAR, phpdocs/PhD_IDE, use --alldeps to download automatically
	phpdocs/PhD can optionally use package "phpdocs/PhD_PEAR"
	phpdocs/PhD can optionally use package "phpdocs/PhD_IDE"
	phpdocs/PhD can optionally use PHP extension "haru"
	phpdocs/PhD_PHP can optionally use PHP extension "haru"
	phpdocs/PhD_Generic can optionally use PHP extension "haru"
	downloading PhD-1.1.10.tgz ...
	Starting to download PhD-1.1.10.tgz (47,401 bytes)
	............done: 47,401 bytes
	downloading PhD_PHP-1.1.10.tgz ...
	Starting to download PhD_PHP-1.1.10.tgz (26,240 bytes)
	...done: 26,240 bytes
	downloading PhD_Generic-1.1.10.tgz ...
	Starting to download PhD_Generic-1.1.10.tgz (29,362 bytes)
	...done: 29,362 bytes
	install ok: channel://doc.php.net/PhD-1.1.10
	install ok: channel://doc.php.net/PhD_PHP-1.1.10
	install ok: channel://doc.php.net/PhD_Generic-1.1.10

(\*1) Unix/Linux/BSD だと、 ./configure で --without-pear とかしない限りデフォルトで入るが、Windows や Mac だと go-pear.phar が必要。詳しくは [PEAR Manual](http://pear.php.net/manual/en/installation.getting.php) を参照  
(\*2) PhD は composer にも対応しているが、依存ライブラリとして何かをすることはないので、PEAR 経由でのインストールが良い

### c) リポジトリからの取得

次にマニュアルのソースをリポジトリから取ってくる。作業用のディレクトリを作って、リポジトリをsvnからチェックアウトする。

下記のURL には、 doc-base, en, ja への外部項目が設定されているので、以下のコマンド一つで3つのリポジトリが一気にチェックアウトできる。

	$ mkdir phpdoc-ja
	$ cd phpdoc-ja
	$ svn co http://svn.php.net/repository/phpdoc/modules/doc-ja .

### d) HTML版の PHPマニュアルをビルドする

--enable-xml-details は、XMLに万が一文法エラーがあったときに細かい情報を出力するためのモノだ。また、phd は結構メモリを使うので、memory_limit の値を大きめにしておいた方がよいかもしれない。

	$ php doc-base/configure.php --with-lang=ja --enable-xml-details
	$ phd -f xhtml -L ja -P PHP -d doc-base/.manual.xml

成功すれば、 output ディレクトリ以下にマニュアルが生成される。

### e) chm 版の PHPマニュアルをビルドする

PHP 7.2.6 では、以下のコマンドで何も考えなくても chm 版をビルドできる。--enable-chm は、chm特有のコンテンツを生成物に含めるために必要だ。 また、--with-lang や -L オプションで言語の指定も忘れずに。

まあ、--enable-chm がなくても、chm特有のヘルプコンテンツが入らないだけで、皆が見たいメインのコンテンツは欠落しないので、問題はないのだけれども。

	$ php doc-base/configure.php --enable-chm --with-lang=ja
	$ phd -f chm -P PHP -L ja -d doc-base/.manual.xml

成功すると、output/php-chm 以下に chm のコンテンツが生成される。

	$ ls output/php-chm/
	php_manual_ja.hhc  php_manual_ja.hhk  php_manual_ja.hhp  res

chm 形式のファイルは LZX 形式で圧縮されており、Linux や Mac では chm 形式は生成できないので、以下、Windows 上で chm ファイルをコンパイルする必要がある。

ここからは Windows での作業だ(\*1) 。まず、[HTML Help Workshop](http://msdn.microsoft.com/en-us/library/ms669985.aspx) をインストールし、output/php-chm/php_manual_ja.hhp を File -> Open で開く。すると、以下のような画面になるはずだ。

![/images/html_help_workshop.png](/images/html_help_workshop.png)

この状態で、File -> Compile を 選択し、php_manual_ja.hhp を以下のようなダイアログで選び、OK を押すと 
php_manual_ja.hhp がある フォルダに php_manual_ja.chm が生成されるはずだ(\*2)。

![/images/html_help_workshop_compile.png](/images/html_help_workshop_compile.png)

(\*1) Windows 10 および、Windows Server 2016 で確認している  
(\*2) 実は、この手順で chm を生成すると、chm の検索が機能しない。きちんと機能させるためには、DBCSFix.exe というアジア系言語のためのバッチを適用し、適切なロケールで HTML Help Compiler を起動して生成する必要がある。これらをWindowsのビルド環境で手当するのは少し大変なので、そこまで手当した生成物が欲しい場合は、[php.net から取得](http://www.php.net/download-docs.php) したほうが良い。詳細を知りたい場合は、[バッチプログラムのソースコード](http://svn.php.net/viewvc/phpdoc/doc-base/trunk/scripts/build-chms.php?revision=333535&view=markup) を読んでみよう
