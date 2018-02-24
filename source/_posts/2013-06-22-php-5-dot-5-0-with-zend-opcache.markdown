---
layout: post
title: "PHP 5.5.0 with Zend OPcache"
date: 2013-06-22 21:51
comments: true
categories: [php]
---
[http://www.php.net/manual/en/book.opcache.php](http://www.php.net/manual/en/book.opcache.php)

PHP 5.5.0 がリリースされた。finally や yield とか ::class によるネームスペースの解決とか楽しい部分はあれど、まー Zend OPcache は大きいよねってことで、とりあえず試しておく。

./configure に --enable-opcache を付けてビルドすればよい。簡単。
あとは php.ini に 以下を追加すると良い。ただし、 extention= ... ではなく、zend_extention= ... と書かねばならないので注意。

	zend_extension=/path/to/php/extensions/no-debug-zts-20121212/opcache.so
	opcache.memory_consumption=128
	opcache.interned_strings_buffer=8
	opcache.max_accelerated_files=4000
	opcache.revalidate_freq=60
	opcache.fast_shutdown=1
	opcache.enable_cli=1

この状態で、 Intel Core i7 2600s と RAM 16GB , Apache 2.2.22 の環境下で [http://www.mumumu.org/~cinnamon/](http://www.mumumu.org/~cinnamon/) (WordPressカスタムサイト) に対して、以下のベンチマークを実行してみた。

	ab -c 10 -n 1000 http://localhost/~cinnamon/

	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/
	
	Benchmarking localhost (be patient)
	Completed 100 requests
	Completed 200 requests
	Completed 300 requests
	Completed 400 requests
	Completed 500 requests
	Completed 600 requests
	Completed 700 requests
	Completed 800 requests
	Completed 900 requests
	Completed 1000 requests
	Finished 1000 requests
	
	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80
	
	Document Path:          /~cinnamon/
	Document Length:        15996 bytes
	
	Concurrency Level:      10
	Time taken for tests:   30.007 seconds
	Complete requests:      1000
	Failed requests:        969
	   (Connect: 0, Receive: 0, Length: 969, Exceptions: 0)
	Write errors:           0
	Total transferred:      16257178 bytes
	HTML transferred:       16001178 bytes
	Requests per second:    33.33 [#/sec] (mean)
	Time per request:       300.066 [ms] (mean)
	Time per request:       30.007 [ms] (mean, across all concurrent requests)
	Transfer rate:          529.09 [Kbytes/sec] received
	
	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    0   0.3      0       4
	Processing:   227  299  35.3    296     663
	Waiting:      208  277  35.3    274     646
	Total:        227  299  35.5    296     667
	
	Percentage of the requests served within a certain time (ms)
	  50%    296
	  66%    305
	  75%    311
	  80%    315
	  90%    329
	  95%    349
	  98%    387
	  99%    423
	 100%    667 (longest request)

PHP 5.4.16 + APC 3.1.13 では以下のようになった。ほとんど変わらない感じだ。

	This is ApacheBench, Version 2.3 <$Revision: 655654 $>
	Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
	Licensed to The Apache Software Foundation, http://www.apache.org/
	
	Benchmarking localhost (be patient)
	Completed 100 requests
	Completed 200 requests
	Completed 300 requests
	Completed 400 requests
	Completed 500 requests
	Completed 600 requests
	Completed 700 requests
	Completed 800 requests
	Completed 900 requests
	Completed 1000 requests
	Finished 1000 requests
	
	Server Software:        Apache/2.2.22
	Server Hostname:        localhost
	Server Port:            80
	
	Document Path:          /~cinnamon/
	Document Length:        15993 bytes
	
	Concurrency Level:      10
	Time taken for tests:   30.171 seconds
	Complete requests:      1000
	Failed requests:        965
	   (Connect: 0, Receive: 0, Length: 965, Exceptions: 0)
	Write errors:           0
	Total transferred:      16283497 bytes
	HTML transferred:       16000497 bytes
	Requests per second:    33.14 [#/sec] (mean)
	Time per request:       301.708 [ms] (mean)
	Time per request:       30.171 [ms] (mean, across all concurrent requests)
	Transfer rate:          527.06 [Kbytes/sec] received
	
	Connection Times (ms)
	              min  mean[+/-sd] median   max
	Connect:        0    0   0.1      0       2
	Processing:   189  301  27.7    297     406
	Waiting:      164  278  27.4    275     381
	Total:        189  301  27.7    297     406
	
	Percentage of the requests served within a certain time (ms)
	  50%    297
	  66%    308
	  75%    315
	  80%    320
	  90%    336
	  95%    353
	  98%    371
	  99%    383
	 100%    406 (longest request)
