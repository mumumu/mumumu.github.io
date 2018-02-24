---
layout: post
title: "Firefox profile setting on Selenium"
date: 2013-05-30 21:02
comments: true
categories: [testing,selenium,firefox]
---
[http://stackoverflow.com/questions/2405714/selenium-and-https-ssl](http://stackoverflow.com/questions/2405714/selenium-and-https-ssl)

[Selenium](http://docs.seleniumhq.org/) を最近テストに愛用している。ただ、https な通信のときにオレオレ証明書を使っている場合、毎回ブラウザが立ち上がるたびに以下のような警告が出て非常にうざいのが問題だった。「危険性を理解した上で接続する」をクリックして例外サイトに加えても結果は同じだったので、これはプロファイルが適用されていないんだな、と気付いた。

![firefox ssl warning](images/firefox_ssl_warning.png)

自分は Selenium Server を以下のような Java のコードで起動し、Firefox をコントロールしていた。これにプロファイルを適用すれば良いことになる。

	protected SeleniumServer seleniumServer;
	protected Selenium selenium;
	
	protected void setUp() throws Exception { 
	
		String testUrl = "http://example.com/foo/bar";
		String testBrowser = "*firefox";
		
		seleniumServer = new SeleniumServer();
		seleniumServer.start();

		selenium = new DefaultSelenium(
				"localhost", seleniumServer.getPort(),
				testBrowser, testUrl
			   );
		selenium.start();
	
	}

上記はコマンドラインで Selenium Server を起動せずに Java から起動しているので、Java からプロファイル設定を渡す方法ってなんだろうと思っていろいろ調べた。だが、コマンドラインから Selenium Server を起動した場合とごっちゃになっていて結論がよくわからなかった。結局以下のように SeleniumServer のコンストラクタに RemoteControlConfiguration オブジェクトを渡せばうまくいった。

	RemoteControlConfiguration rcc = new RemoteControlConfiguration();
	rcc.setFirefoxProfileTemplate(new File("/path/to/firefox_profile_dir"));
	seleniumServer = new SeleniumServer(rcc);

Firefox のプロファイルのディレクトリはプラットフォームによって異なる。Windows であれば、%AppData%/Roaming/Mozilla/Firefox/Profiles/xxxxxxxxxx となる。

これでプロファイルを Selenium Server に渡せるようになったのはいいのだが、Firefox の起動が極めて重かったので参った。プロファイルを渡さなかったときは重くなかったので、別に空のプロファイルを作り、そのプロファイルにオレオレ証明書を受け入れさせたら軽くなった。 Firefox で新しいプロファイルを作るには、以下のコマンドを使う。

	firefox -P

そうすれば以下のようなウィンドウが起動するので、プロファイルを設定し、Java 側で新たに指定すればよい。

![firefox profile window](images/firefox_profile_window.png)

Selenium Server をコマンドラインから起動する場合は、以下のやり方でプロファイルを渡すことが出来る。このやり方なら、 Java 側で SeleniumServerクラスをインスタンス化して start する必要はない。

	java -jar selenium-server-standalone-2.31.0.jar -firefoxProfileTemplate /path/to/firefox_profile_dir

ヤレヤレ。少し面倒だが、だいぶ楽になった(´ー｀; )
