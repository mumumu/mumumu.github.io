---
layout: post
title: "Yet Another PEP8 Japanese version"
date: 2014-03-26 22:20
comments: true
categories: [python, translation]
---

[https://github.com/mumumu/pep8-ja](https://github.com/mumumu/pep8-ja)

Python のコーディングスタイルのガイドとしてよく知られている PEP8 の日本語訳を公開した。

もともと、社内で PEP8 に従ってコードを書こう (もともと [flake8](https://pypi.python.org/pypi/flake8) は採用しているのだけど、ローカルルールとかも含めて明示したいねという話) と言っているときに、日本語版があった方が話が早いと思ったのが翻訳を求めたきっかけである。既存の翻訳があればそれに越したことはなかったのだが、自分がお勧めできるクオリティの翻訳が存在しなかったので、あえて再発明に踏み切った次第だ。

とはいえ、[@methane さんの翻訳](https://dl.dropboxusercontent.com/u/555254/pep-0008.ja.html) は一部参考にさせて頂いている。特に最後の[関数のアノテーション](http://docs.python.jp/3.3/tutorial/controlflow.html#function-annotations) に関する部分は Python 3.0 以降で有効な機能であり、自分も使ったことがないため正直曲者だった。ちゃんとPythonを書くようになっていろいろ学ぶことは多いが、広く使われているプラクティスの一覧という意味で、この翻訳からは得るものが大いにあった。社内でもこれを生かしていきたいし、他の方のお役に立つことがあれば嬉しい。

github にあがっているので、異論反論オブジェクションについては、github Issue か Pull Request をお願いします(*´～｀)
