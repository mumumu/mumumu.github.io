---
layout: post
title: "RapidSSL(2) - HeartBleed related"
date: 2014-04-13 23:03
comments: true
categories: [security]
---
[http://heartbleed.com/](http://heartbleed.com/)

以前 [RapidSSL の証明書を遊びで取得した](http://mumumuorg.blogspot.jp/2012/08/rapidssl.html) けれども、RapidSSL から以下のようなメールが来た。彼らも真っ当なSSL業者の端くれだったということか。

```
RapidSSL
Important Service Notification

Dear Yoshinari Takaoka,

RapidSSL is aware of the vulnerability, dubbed &ldquo;Heartbleed&rdquo;, which is a security concern for
users of OpenSSL, a widely-used opensource cryptographic software library. It can allow attackers
to read the memory of the systems using vulnerable versions of OpenSSL library (1.0.1 through 
1.0.1f). This may disclose the secret keys of vulnerable servers, which allows attackers to 
decrypt and eavesdrop on SSL encrypted communications and impersonate service providers. In
addition, other data in memory may be disclosed, which conceivably could include usernames and
passwords of users or other data stored in server memory.

(...以下略。鍵を含めた証明書の更新と古い証明書の失効化を求める内容)
```

基本的には [手順に従って秘密鍵を含めた証明書の更新](https://knowledge.rapidssl.com/support/ssl-certificate-support/index?page=content&id=AD834) をすればよいが、秘密鍵も含めたSSL通信の内容が漏れていることが前提なので、**既存の秘密鍵や CSR を絶対に再利用してはいけない**点は注意。その点は問題なくできたのだが、RapidSSL は古い証明書の失効化がオンラインで出来なかったので、以下にその手順をメモしておく。

a) OpenSSL をアップデートする  
b) 古い証明書のシリアルナンバーと Common Name, 発行日時を以下のコマンドでメモする。

```
$ openssl x509 -in server.pem.old -text
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 520431 (0x7f0ef)
    Signature Algorithm: sha1WithRSAEncryption
        Issuer: C=US, O=GeoTrust, Inc., CN=RapidSSL CA
        Validity
            Not Before: Aug 24 08:43:44 2012 GMT
            Not After : Aug 27 02:46:21 2014 GMT
        Subject: (.snip) CN=www.mumumu.org
```

c) 証明書を更新してインストールする  
d) RapidSSL の Order ID と 上記 a) の情報を証明を取得した業者にメールする。自分の場合は [RapidSSL JP](http://www.rapid-ssl.jp/) から取得したので、そこに連絡しておいた。[彼らは失効化に関する情報を公開している](http://www.rapid-ssl.jp/rapidssl-news/archives/55)  
d-1) 上記の注意点として、特にシリアルナンバーを間違えると間違った証明書が失効してしまう。また、証明書を更新せずに今使っている証明書を失効化してしまうと再発行すらできなくなってしまう。マジで注意。  

オンラインでできると思ったけど、意外に面倒だったなぁ。という話(*´～｀)
