---
layout: post
title: "PHP manual generate HOWTO (version 2013-06-29)"
date: 2013-06-29 16:16
comments: true
categories: [php,howto]
---
[http://d.hatena.ne.jp/anatoo/20120430/1335725004](http://d.hatena.ne.jp/anatoo/20120430/1335725004)

このエントリは、anatoo氏 による上記エントリの更新版である。  
大筋は変わっていないが、細かい箇所を補足しているのと、chm に関する記述が少し増えている。

----
<br />
PHPのマニュアルは、DocBook と呼ばれるフォーマットで記述されており、[PhD という PHPマニュアル のためのツール](https://wiki.php.net/doc/phd) によってその DocBook から HTML や CHM、PDF などの複数の形式にビルドできる。

この記事では、2013年6月末時点で実際にPHPのマニュアルをリポジトリから取ってきてビルドする方法を紹介する。PhD は PDF や manpage形式、epub 等多様な形式がビルドできるが、ここでは HTML および chm のみについて触れる。

### a) ビルドするときに使うPHPについて

まず、PHP をインストールしなければはじまらない。  
PHP は 5.3.x が望ましい。今は 5.4.x や 5.5.0 とかが出ているが、Windows 用ヘルプファイル(chm)をビルドするときに、 5.3 でないと日本語の情報が欠落するバグが PhD には存在するからだ(後述)

chm をビルドするつもりがないなら、PHP 5.4, 5.5 でも差し支えない。
PHP をインストールするときも、PhD のために DOM, libXML2, XMLReader and SQLite3 は有効にしていなければならない。ただ、これらは 5.3 以降はデフォルトで有効になっているので、無効にしないよう注意しよう。

### b) 必要なツールをインストール

subversionとpearを予めインストールしておいて、PhD と PhD_PHP をpear経由でインストールする。  
PhD_PHP は chm をビルドするのに必要だ。

	$ pear version
	PEAR Version: 1.9.4
	PHP Version: 5.5.0
	Zend Engine Version: 2.5.0-dev
	$ pear install doc.php.net/PhD doc.php.net/PhD_PHP
	phpdocs/PhD can optionally use package "phpdocs/PhD_PEAR"
	phpdocs/PhD can optionally use package "phpdocs/PhD_IDE"
	phpdocs/PhD can optionally use PHP extension "haru"
	phpdocs/PhD_PHP can optionally use PHP extension "haru"
	phpdocs/PhD_PHP can optionally use PHP extension "bz2"
	phpdocs/PhD_Generic can optionally use PHP extension "haru"
	downloading PhD-1.1.6.tgz ...
	Starting to download PhD-1.1.6.tgz (47,079 bytes)
	.............done: 47,079 bytes
	PHP Deprecated:  Assigning the return value of new by reference is deprecated in /home/mumumu/.phpenv/versions/5.5.0/lib/php/PEAR/PackageFile/v2/Validator.php on line 1740
	
	Deprecated: Assigning the return value of new by reference is deprecated in /home/mumumu/.phpenv/versions/5.5.0/lib/php/PEAR/PackageFile/v2/Validator.php on line 1740
	downloading PhD_PHP-1.1.6.tgz ...
	Starting to download PhD_PHP-1.1.6.tgz (26,028 bytes)
	...done: 26,028 bytes
	downloading PhD_Generic-1.1.6.tgz ...
	Starting to download PhD_Generic-1.1.6.tgz (29,064 bytes)
	...done: 29,064 bytes
	install ok: channel://doc.php.net/PhD-1.1.6
	install ok: channel://doc.php.net/PhD_PHP-1.1.6
	install ok: channel://doc.php.net/PhD_Generic-1.1.6
	
### c) リポジトリからの取得

次にマニュアルのソースをリポジトリから取ってくる。作業用のディレクトリを作って、リポジトリをsvnからチェックアウトする。

下記のURL には、 doc-base, en, ja への外部項目が設定されているので、以下のコマンド一つで3つのリポジトリが一気にチェックアウトできる。

	$ mkdir phpdoc-ja
	$ cd phpdoc-ja
	$ svn co http://svn.php.net/repository/phpdoc/modules/doc-ja .

### d) HTML版の PHPマニュアルをビルドする

--enable-xml-details は、XMLに万が一文法エラーがあったときに細かい情報を出力するためのモノだ。また、phd は結構メモリを使うので、memory_limit の値を大きめにしておいた方がよいかもしれない。

	$ php doc-base/configure.php --with-lang=ja --enable-xml-details
	$ phd -d doc-base/.manual.xml

成功すれば、 output ディレクトリ以下にマニュアルが生成される。

### e) chm 版の PHPマニュアルをビルドする(PHP 5.3.x)

PHP 5.3.x ならば、以下のコマンドで何も考えなくても chm 版をビルドできる。--enable-chm は、chm特有のコンテンツを生成物に含めるために必要だ。 また、--with-lang や -L オプションで言語の指定も忘れずに。

まあ、--enable-chm がなくても、chm特有のヘルプコンテンツが入らないだけで、皆が見たいメインのコンテンツは欠落しないので、問題はないのだけれども。

	$ php doc-base/configure.php --enable-chm --with-lang=ja
	$ phd -f chm -P PHP -L ja -d doc-base/.manual.xml

成功すると、output/php-chm 以下に chm のコンテンツが生成される。

	$ ls output/php-chm/
	php_manual_ja.hhc  php_manual_ja.hhk  php_manual_ja.hhp  res

chm 形式のファイルは LZX 形式で圧縮されており、Linux や Mac では chm 形式は生成できないので、以下、Windows 上で chm ファイルをコンパイルする必要がある。

ここからは Windows での作業だ。まず、[HTML Help Workshop](http://msdn.microsoft.com/en-us/library/ms669985.aspx) をインストールし、output/php-chm/php_manual_ja.hhp を File -> Open で開く。すると、以下のような画面になるはずだ。

![/images/html_help_workshop.png](/images/html_help_workshop.png)

この状態で、File -> Compile を 選択し、php_manual_ja.hhp を以下のようなダイアログで選び、OK を押すと 
php_manual_ja.hhp がある フォルダに php_manual_ja.chm が生成されるはずだ。

![/images/html_help_workshop_compile.png](/images/html_help_workshop_compile.png)

### f) chm 版の PHPマニュアルをビルドする(PHP 5.4.x以降)

既に述べた通り、PHP 5.4 以降で PhD を使うと、chm のセクションタイトル情報が欠落するバグがある。これについては[既に修正コードが投稿されている](https://github.com/php/phd/pull/3) のだがまだ取り込まれていない。よって、PHP 5.4 以降で chm をビルドしたければ、[修正されている git のコード](https://github.com/mumumu/phd) を使う。

まずは、既にインストールされている PhD をアンインストールする

	$ pear uninstall doc.php.net/PhD doc.php.net/PhD_Generic doc.php.net/PhD_PHP
	uninstall ok: channel://doc.php.net/PhD_PHP-1.1.6
	uninstall ok: channel://doc.php.net/PhD_Generic-1.1.6
	uninstall ok: channel://doc.php.net/PhD-1.1.6

修正版の git の PhD をインストールする

	$ git clone https://github.com/mumumu/phd.git 
	$ cd phd
	$ pear install package.xml package_generic.xml package_php.xml 
	phpdocs/PhD can optionally use package "phpdocs/PhD_PEAR"
	phpdocs/PhD can optionally use package "phpdocs/PhD_IDE"
	phpdocs/PhD can optionally use PHP extension "haru"
	phpdocs/PhD_Generic can optionally use PHP extension "haru"
	phpdocs/PhD_PHP can optionally use PHP extension "haru"
	phpdocs/PhD_PHP can optionally use PHP extension "bz2"
	install ok: channel://doc.php.net/PhD-1.1.7
	install ok: channel://doc.php.net/PhD_Generic-1.1.7
	install ok: channel://doc.php.net/PhD_PHP-1.1.7
	$ cd ..

あとは普通にビルドし、既に述べたとおりのやり方で、Windows 上で chm をコンパイルすると良い。

	$ php doc-base/configure.php --enable-chm --with-lang=ja
	$ phd -f chm -P PHP -L ja -d doc-base/.manual.xml
