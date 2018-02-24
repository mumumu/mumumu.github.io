---
layout: post
title: "Date Calculation"
date: 2013-05-31 10:44
comments: true
categories: [programming]
---
[http://docs.oracle.com/javase/7/docs/api/java/util/Calendar.html](http://docs.oracle.com/javase/7/docs/api/java/util/Calendar.html)

以下のように日付を足したり引いたりして、その参照を使い回すと極めて危険である。  
... っていうネタはあるあるだと思うんですけど、今日そのワナを踏んでしまって泣きそう( ;´Д⊂ヽ   
これは Java の例だけれども、当然 Java に限った話ではない。

	Calendar now = Calendar.getInstance();  // instance of 5/31
	now.add(Calendar.MONTH, -3);           // 3 month before -> 2/28
	
	//  some processing...
	
	now.add(Calendar.MONTH, +3);           // not 5/31, but 5/28 !!!

要するに、たまたま3ヶ月前が2月末だったので、ずれるという話です。  
こんな馬鹿なことするのは俺だけだって？ デスヨネー(´ー｀; )
