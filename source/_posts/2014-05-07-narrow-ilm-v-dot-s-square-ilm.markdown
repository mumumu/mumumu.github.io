---
layout: post
title: "Narrow ILM v.s Square ILM"
date: 2014-05-07 00:15
comments: true
categories: [memo, hardware]
---
[http://www.servethehome.com/narrow-ilm-square-ilm-lga2011-heatsink-differences/](http://www.servethehome.com/narrow-ilm-square-ilm-lga2011-heatsink-differences/)

自作をやった人なら御存知かもしれないが、Intel な CPU をマザーボードのCPUソケットに固定するための部品を ILM (Independent Loading Mechanism) という。手元にあるマザーボードだと、以下の長方形の部分になる。レバーを上げてCPUをおいて、カバーを付けてレバーを下げると固定されるという具合だ。

<img src="/images/ilm.jpg" width="480" height="320" />

今回自分が購入したマザーボードの CPUソケットは LGA 2011 である。LGA 2011 の ILM には二つの種類がある。Square ILM と Narrow ILM だ。

a) Square ILM  
　- サイズ 80x80mm の正方形  
　- コンシューマー向けのマザーボードのソケットのほとんどはこれ  
b) Narrow ILM  
　- サイズ 56x94mm  
　- マザーボードに載せる機能が多いため、ヒートシンクの面積を制限する理由があるサーバ向けのマザーボードに採用例が多い  

これらの何が違うかというと、ソケットの上に載せることになるヒートシンクのサイズである。ほとんどの人が自作に使うマザーボードについては、Square ILM が前提になっており、ヒートシンクもそれを前提にして LGA 2011 対応とのみ謳っている。これがフツーのヒートシンクである。

だが、今回自分が購入した [GIGABYTE GA-6PXSV4](http://b2b.gigabyte.com/products/product-page.aspx?pid=4466) については、Narrow ILM が採用されていたため、フツーのヒートシンクが付かなかった。。 [Katana4](http://www.scythe.co.jp/cooler/katana4.html)  というクーラーを買っていたのだが、これも Square ILM を前提にしていたのである。

Narrow ILM はこのような性格から情報が少ない上、国内で Narrow ILM 対応と謳って売っているヒートシンクも少ない傾向にある。[オリオで売っている Noctua のやつ](http://www.oliospec.com/index.php?action=item_search&rootCategoryId=161) くらいだろうか。また、機能を詰め込んだマザーボードに採用されているため、推奨するヒートシンクの面積すら指定されているマザーボードだとますます適合品が少なくなる傾向にある。

自分が買った Gigabyte GA-6PXSV4 だと、[「Recommended cooling device dimension: 70x106mm」となっていた](http://b2b.gigabyte.com/products/product-page.aspx?pid=4466#sp) ため 泣く泣くそれに適合する [Dynatron のヒートシンク](http://www.dynatron-corp.com/en/product_detail_1.aspx?cv=&id=238&in=0) を発注するハメになった。

## まとめ

・ Supermicro とかのサーバ向けマザーボードでは、ILM のサイズに注意  
・ ILM に適合するヒートシンクを発注しよう！ 但しコンシューマー向けマザーボードでは気にする必要はない。  
・ 参考文献  
　- [Narrow ILM v. Square ILM – LGA2011 Heatsink Differences](http://www.servethehome.com/narrow-ilm-square-ilm-lga2011-heatsink-differences/)  
　- [Xeon E5-1600/2600/4600 v1 and v2 Thermal / Mechanical Design Guide(PDF)](http://www.intel.com/content/dam/www/public/us/en/documents/design-guides/xeon-e5-1600-2600-4600-thermal-guide.pdf)
