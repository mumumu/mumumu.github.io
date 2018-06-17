---
layout: post
title: "Current status of the PHP Documentation"
date: 2013-12-08 00:00
comments: true
categories: [documentation, php]
---

この記事は、[doc-ja Advent Calendar](http://qiita.com/advent-calendar/2013/docja) 8日目の参加記事です。

ここでは、PHPを使うと必ず一度は目にするであろう [PHP マニュアル](http://www.php.net/manual/ja/) についてつれづれと書いてみようと思います。「PHP のドキュメント」に関する日本語の文書が少なくなってきている気がすることが、このエントリを書く動機になっています。

**[更新履歴]**  

1) 2013/12/08 初版  
2) 2018/06/17 edit.php.net の記述を現状に合わせて置き換え、PHP manual generate howto を最新版のリンクに置き換えた

### PHPマニュアル の現状をざっくりと

PHP マニュアル はPHP 4系から現在 (このエントリを書いている時の最新は5.5.6) に至るまでの歴史が蓄積されており、非常に巨大なマニュアルです。これは [DocBook](http://www.docbook.org/) を使って書かれており、それを適宜各ロケールの翻訳者がボランティアで日本語に置き換えていっています。それが php.net のインフラでビルドされ、週の一度 (正確には毎週金曜日の10:00 UTC) 世界中に公開されていくという具合です。

Docbook は、SGML 由来であることによって文書構造が複雑であることや、タグの構造が壊れてしまうと即ビルドが失敗してしまうなどの欠点によって、他のフォーマットに置き換えようという話がこれまで出なかったわけではありません。ですが、DocBook の多彩な表現力にとってかわることと、現在までの巨大な蓄積を効率的に置き換えられる代替案を誰かが示せない限り、移行していくことはないでしょう。

バージョン管理システムは [Subversion](http://subversion.apache.org/) です。えー今の御時世に(ry という声が聞こえてきそうですが、これも Docbook と同じく強いモチベーションと代替案を示すことができる誰かがいないと、というところでしょう。

### PHP マニュアル に間違いを見つけたら？

既に述べたとおり、PHP マニュアル はボランティアベースで作られているため、常に完全な翻訳を維持できているわけではありません。現在でもたくさんの未訳部分が残っていますし、人間がやるモノである以上、常に間違いが存在し得ます。

[日本 PHP ユーザー会](http://www.php.gr.jp) では [php-doc ML](http://ml.php.gr.jp/mailman/listinfo/php-doc) というメーリングリストをホストしており、ここを通じて PHPマニュアルに関する問い合わせや間違いの指摘などを受け付けています。ここで頂く多くの情報が、PHP マニュアル 日本語版の質の向上に大きく寄与していることは言うまでもありませんし、初期の頃からの多くの方々の協力によって現在のマニュアルは存在し、かつ維持されています。

また、[PHP のバグトラッカー](http://bugs.php.net/) で Documentation Problem と記して英語でバグ報告をするのもアリですが、こちらは気付かれないことも多いので php-doc ML の方をドキュメントの指摘についてはお勧めしています。

### edit.php.net について

[edit.php.net ](https://edit.php.net/) は、PHPマニュアルのオンラインエディタです。コミット権限を持っていない人でもソースの修正ができます。修正すると各国語のコミッタに修正が伝わるので、ML で話を通さずに手軽に修正したい場合は、このツールを使うのもアリだと思います。

**[ Update June 17th 2018 21:43 JST by m ]**

2013年当時は、英語以外のロケールを編集しても各国語のコミッタには通知がなかったので、「使い勝手はイマイチ」と説明していましたが、その点は現在は改善されているため、関連する記述を削除して置き換えました

### 対応したフォーマット

PHP マニュアル は HTML や Windows の標準的なヘルプ形式である [chm](http://ja.wikipedia.org/wiki/Microsoft_Compiled_HTML_Help) に対応したフォーマットが公開されていますが、Docbook からこれらの形式をビルドするための [PhD](https://wiki.php.net/doc/phd) というツールが整備されています。これを使えばHTML形式のビルドは行えるようになっていますが、chm 形式については、それをビルドするのに Windows マシンの助けが必要です。PHP マニュアル のビルド方法については、以下に纏めておいたので参考にしてください。

[PHP Manual Generate Howto(2018年6月16日版)](/blog/2018/06/16/php-manual-generate-howto/)

**[ Update June 17th 2018 21:43 JST by m ]**

上記の Howto を、2013年版から最新版に置き換えました

### PHP マニュアル のコミット権限を得るには？

PHPマニュアルをいろいろ見ていくと、継続して改善すべきだというモチベーションが強まってくるかもしれません。PHP-doc ML で指摘しまくっていてそれにも飽き足らないという人は、コミット権限を得て自分で直すことも考えてもよいかもしれません。以下は、そういうモチベーションが高い人向けです。

PHPマニュアルのコミット権限を得るには、[php.net のページで英語で申請をする](http://www.php.net/git-php.php) 必要があります。これを php.net の中の人が見てコミット権限を与えるかを決めているわけですが、彼らは何回か patch を提出した実績か、もしくは既存のコミッタの推薦があることを求めて来ます。patch については日本語版の PHP マニュアル の patch でも問題ないはずです。

基本的には [php-doc ML](http://ml.php.gr.jp/mailman/listinfo/php-doc) に話を通してから申請し、既存の PHP マニュアル 日本語版のコミッタからの推薦を得た方が早く話が進むと思います (自分もそうやって [高木さん](http://d.hatena.ne.jp/takagimasahiro) の推薦を貰いました) 。

尚、申請ページでは、最初から最後までちゃんと文章を読んでから申請フォームを埋めないと **必ず失敗するようになっています** ので気を付けてください。

#### (番外編) chmファイルが壊れたのでホストを引き継いだ話

さて、ここからちょっと本題からずれます。

PHPマニュアル の chm 版をビルドするには、上で紹介したとおり Windows の助けが必要です。よって、chmファイルは世界のどこかのボランティアの Windows マシンでビルドされ、それを php.net 側が吸い取って世界中に公開しているわけです。

今年の5月あたりから、この世界のどこかにあるボランティアのマシンが止まったらしく、[chmファイルが壊れている](https://bugs.php.net/bug.php?id=64842) というバグレポートが届くようになりました。再現させようとしてみたところ、確かに[ビルドシステムの一部が壊れていたのでそれを直し](http://svn.php.net/viewvc/phpdoc/doc-base/trunk/scripts/build-chms.php?r1=330908&r2=330907&pathrev=330908)、流石に Windows マシンをホストしている側がこのエラーに気付かんわけはあるまい、とその後は放置していました。

その後、私事でばたばたしたりなどしていて10月になりました。たまたま本家のメーリングリストを見ていると、「chm ファイルはどうなってるんだ？」 という問い合わせがあったのが目に付きました。

```
> On Fri, Oct 4, 2013 at 2:00 AM, Marvin D. <marvin@phpugph.com> wrote:
> > To whom it may concern,
> >
> > I just went to the downloads section of the php manual, and I can't seem
> > to find any downloadable file that is in ".chm" file format. when will that
> > format be available for downloads of the php manual?. 
> 
> We no longer have access to a Windows machine to build the CHM format
> on, so we had to discontinue it.
> Sorry
```

「chm ファイルのビルドは止まってるから配布不能だ」としれっとメインの人が答えているのを見て驚愕。 いったいこいつは何を言ってるんだ？ どうやら彼らの管理下にマシンはない模様。であれば、自分が chm のビルドやるよ、と言っても文句は出ないよね。ということで最近引き継ぎました。

基本は上で出したリンクでの [PHP Manual Generate Howto](http://mumumu.github.io/blog/2013/06/29/php-manual-generate-howto/) を Windows でそのままやっただけですが、週に一度定期的にビルドしてアップロードする、という箇所を自動でやらないといけなかったので、そこは Dropbox と Jenkins で補完しました。 php.net の人たちはこれらを Windows でやるのが大変 Annoying なタスクと言ってましたが、確かに2000年代だとそうだったでしょう。2013 年には自動 sync やビルドツールが進化してくれていて大変助かりました(´ー｀; )

そんな感じで、日本の片隅でビルドした成果物が世界中に配られているわけです。翻訳をやる動機の一つって、翻訳の広がりをいろんな点で感じられることにあると個人的には思っているんですが、こうやってインフラを支えるのもそのひとつだね。と思いました。まる。

### まとめ

・ 日本語PHPマニュアルの現状を紹介  
・ マニュアルに間違いがあったら [php-doc ML](http://ml.php.gr.jp/mailman/listinfo/php-doc) へ！！！１  
・ インフラを支えることを手伝うのもまた良いことだ  
