---
layout: post
title: "phpdoc メンテナンス業を少し強化する気になった"
date: 2019-12-24 21:30:11 +0900
comments: true
categories: [php,documentation]
---
自分は PHP マニュアルのコミット権限を持っているが、[2013年に chm のビルドインフラを引き継いだ時](http://mumumu.github.io/blog/2013/12/08/current-status-of-the-php-documentation/) 以来、その権限をまともに行使したことはあまりなかった。実際 [自分が対応した chm のビルドインフラのバグ報告は4回](https://bugs.php.net/search.php?search_for=chm&boolean=0&limit=30&order_by=&direction=DESC&cmd=display&status=Closed&bug_type=All&project=All&php_os=&phpver=&cve_id=&assign=mumumu&author_email=&bug_age=0&bug_updated=0&commented_by=) しかないし、phpdoc 本体のコミット数も 2013年 - 2018年までの間を数えてみると 10 回あるかないかだ。

今年になり、PHPマニュアル自体があまりメンテナンスされてないっぽいな、という感覚になった。なぜかというと、高木さんや吉田さんなど、これまでよくコミットログで見かけた人たちの作業ログを見かけなくなったからである。

それでも見渡すと、日本語ロケールで最新の更新を維持しているのは全体の6割であり、少し手を加えれば最新に更新できるものを含めると7割にも達する。現状でも全ロケールを並べるとトップレベルにある。

<img src="/images/phpdoc-all-locale-stats.png"/>
<img src="/images/phpdoc-translation-stats-detail-ja.png"/>

「PHPマニュアル自体があまりメンテナンスされてないっぽいな、という感覚」を踏まえてもこの状況なのはかなり驚異的であり、先人の蓄積に正直驚いた。頭を垂れるしかない。

PHP の拡張機能は増加の一途を辿っているし、ChangeLog を正しい状態に維持するだけでも大変な状態(※ 注1) なので、皆が読む部分だけでも維持するのを手伝えればな、と思う。 少なくとも「少し手を加えれば最新になる」部分については。

ということで、ここ2週間の作業で、reference/ 以外の翻訳はとりあえず最新にした。  
力を入れず、「皆が読む部分の最新を維持できれば良い」程度の力の入れ具合で行きたい。

あと、もっと多くの人を巻き込むべく、[PHPユーザーズの Slack](https://phpusers-ja.slack.com/) でも #phpmanual というチャンネルを作ってもらった。[phpdoc の git への移行もゆっくりながら進んでいる](https://news-web.php.net/php.doc/969387428) ので、さらに多くの人を巻き込めるようにしておきたい。

(※ 注1) 業を煮やした cmb が、[PHP 5.4.0 より前のマニュアル消そうぜ、という提案を最近する](https://news-web.php.net/php.doc/969387407) 程度には大変である。
