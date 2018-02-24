---
layout: post
title: "'X not in Y' or 'not X in Y'?"
date: 2013-09-12 13:29
comments: true
categories: python
---
[http://docs.python.jp/2/reference/expressions.html#summary](http://docs.python.jp/2/reference/expressions.html#summary)

Python で Y (辞書またはリスト) に X という要素やキーが「存在しない」ことをどう書くか。 
Python では以下の2通りが書ける。

A) not X in Y  
B) X not in Y  

現に両方の書き方をしている人が社内ではいた。そこで「どっちかに統一した方がよくね?」という話になった。

A) は "in 演算子" を "not 演算子" で否定したもので、B) は "not in 演算子" をそのまま使ったものである。社内では B) の書き方をしている人が多数を占めた。理由としては以下のようにいくつか考えられる。

a) 演算子の優先順位を考えなくて良い  
b) 英語としても自然  
c) SQL にも not in があるので馴染みがある  

個人的には a) が一番大きい。in を not で否定するというのも読めなくもないけど、"not in" がひとつの演算子なんだ、という認識を c) からもしているので、やはり B) が読みやすい。
