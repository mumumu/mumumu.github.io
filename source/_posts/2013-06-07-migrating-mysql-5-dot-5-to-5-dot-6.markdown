---
layout: post
title: "Migrating MySQL 5.5 to 5.6"
date: 2013-06-07 12:47
comments: true
categories: [mysql]
---
[MySQL](http://www.mysql.com/)5.5 からビルドは [cmake](http://www.cmake.org) でするようになったが、ビルドオプションが手元に残らない（誰か [Autotool](http://www.gnu.org/software/automake/) でいうところの config.status みたいなものがあれば教えて!)。そして、MySQL の新しいメジャーバージョンが出る度に公式ドキュメントの一般的な手順を追うのも面倒臭い。

ということで、以下に私的な Debian GNU/Linux Wheezy 上でのアップグレード手順をメモしておく。家サーバの私的な手順なので参考にしてはいけない。ソースから入れているので apt-get install build-essential も当然しておくこと。

また、mysql ユーザーとグループは作成済みとし、起動スクリプト /etc/init.d/mysqld も作成済みとする。

----
<br />
マニュアルを読み、アップグレードに関する注意や非互換変更、リリースノートを読んでおく。

[http://dev.mysql.com/doc/refman/5.6/en/upgrading.html](http://dev.mysql.com/doc/refman/5.6/en/upgrading.html)
[http://dev.mysql.com/doc/refman/5.6/en/upgrading-from-previous-series.html](http://dev.mysql.com/doc/refman/5.6/en/upgrading.html)
[http://dev.mysql.com/doc/refman/5.6/en/mysql-upgrade.html](http://dev.mysql.com/doc/refman/5.6/en/upgrading.html)
[http://dev.mysql.com/doc/relnotes/mysql/5.6/en/](http://dev.mysql.com/doc/refman/5.6/en/upgrading.html)

次に、ソースアーカイブを展開して普通にビルドする。

	$ tar zxvf mysql-5.6.12.tar.gz
	$ cd mysql-5.6.12
	$ cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql5.6 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DENABLED_LOCAL_INFILE=ON -DWITH_READLINE=ON  .
	$ make -j8
	$ sudo make install
	$ sudo -s

ここで root になり、/usr/local/mysql5.6 以下の権限を設定する。  
最後の data ディレクトリへの設定は意味ないが、様式美のようなものだ。

	# cd /usr/local/mysql5.6
	# chown -R root.mysql .
	# chown -R mysql data

mysqld を一度停止する。

	# /etc/init.d/mysqld stop

自分はデータを /home/mysql/data-VERSION 以下に置いているので、それを新たな場所にコピーし、権限を設定する。元データを残しておくのが mysqldump 等によるバックアップの代替だ。

	# cd /home/mysql
	# cp -pR data-5.5 data-5.6
	# chown -R mysql.mysql data-5.6

/etc/init.d/mysqld を編集する。自分は support-files/mysql.server をそのまま使っているので、basedir と datadir を以下のように書き換える。

	--- mysqld-5.5  2013-06-07 12:58:13.075107498 +0900
	+++ mysqld      2013-06-07 12:37:57.775074031 +0900
	@@ -43,8 +43,8 @@
	# If you change base dir, you must also change datadir. These may get
	# overwritten by settings in the MySQL configuration files.
	 
	-basedir=/usr/local/mysql5.5
	-datadir=/home/mysql/data-5.5
	+basedir=/usr/local/mysql5.6
	+datadir=/home/mysql/data-5.6

MySQL を起動する。(この時点で既に5.6)

	# /etc/init.d/mysqld start

一度接続できることを確認する。

	# /usr/local/mysql5.6/bin/mysql -u root -p mysql
	Enter password: 
	Welcome to the MySQL monitor.  Commands end with ; or \g.
	Your MySQL connection id is 85
	Server version: 5.6.12-log Source distribution
	
	....

mysql_upgrade を実行する。 --basedir と --datadir は無視されるんだそうな。  
[マニュアル通りですな。](http://dev.mysql.com/doc/refman/5.6/en/mysql-upgrade.html)

	# /usr/local/mysql5.6/bin/mysql_upgrade --basedir=/usr/local/mysql5.6/ --datadir=/home/mysql/data-5.6/ -u root -p
	/usr/local/mysql5.6/bin/mysql_upgrade: the '--basedir' option is always ignored
	/usr/local/mysql5.6/bin/mysql_upgrade: the '--datadir' option is always ignored
	Enter password: 
	Looking for 'mysql' as: /usr/local/mysql5.6/bin/mysql
	Looking for 'mysqlcheck' as: /usr/local/mysql5.6/bin/mysqlcheck
	Running 'mysqlcheck' with connection arguments: '--port=3306' '--socket=/tmp/mysql.sock' 
	Warning: Using a password on the command line interface can be insecure.
	Running 'mysqlcheck' with connection arguments: '--port=3306' '--socket=/tmp/mysql.sock' 
	Warning: Using a password on the command line interface can be insecure.
	mysql.columns_priv                                 OK
	mysql.db                                           OK
	mysql.event                                        OK
	mysql.func                                         OK
	mysql.general_log                                  OK
	mysql.help_category                                OK
	mysql.help_keyword                                 OK
	mysql.help_relation                                OK
	mysql.help_topic                                   OK
	mysql.host                                         OK
	mysql.ndb_binlog_index                             OK
	mysql.plugin                                       OK
	mysql.proc                                         OK
	mysql.procs_priv                                   OK
	mysql.proxies_priv                                 OK
	mysql.servers                                      OK
	mysql.slow_log                                     OK
	mysql.tables_priv                                  OK
	mysql.test                                         OK
	mysql.time_zone                                    OK
	mysql.time_zone_leap_second                        OK
	mysql.time_zone_name                               OK
	mysql.time_zone_transition                         OK
	mysql.time_zone_transition_type                    OK
	mysql.user                                         OK
	Running 'mysql_fix_privilege_tables'...
	....
	OK

これで完了。何か起きたときは /etc/init.d/mysqld を古いモノに差し替えて mysqld を再起動すること。	
