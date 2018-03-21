---
layout: post
title: "Silly patch - could not detect repository update"
date: 2015-12-26 03:06
comments: true
categories: [patch]
---
PEP8 のファイル更新を監視するのに、 [Mercurial リポジトリ更新チェックツール](https://github.com/mumumu/mercurial_file_update_checker) というのを書いている。コミットメールや [hg.python.org](https://hg.python.org/) の RSS を見れたりすれば一番良いのだが、前者はPEPの編集者や開発者にしか開放されておらず、後者はファイル単位の更新を見せるようにはできていない(\*1) 。このツールはファイル単位での更新があったらメールしてくれるというもので、自分のニーズを満たすための単純なものだ。

ところがこのツール、ちゃんと動いていなかった。更新がリモートリポジトリにあってもちゃんと検知できていなかったのだ。ダメぢゃん！  
原因は [git pull にあたるコマンド](https://www.mercurial-scm.org/wiki/GitConcepts#Command_equivalence_table) を hg update だと思い込んでいたというもので、単純に Mercurial の理解不足である。

なので、こういうお馬鹿な patch を書いてしまうんですね。ハイ(´ー｀; )  
誰の役にも立たないんで、ますますお馬鹿度が増すという...

```diff
diff --git a/rev_update_checker.py b/rev_update_checker.py
index b6109fe..af39899 100644
--- a/rev_update_checker.py
+++ b/rev_update_checker.py
@@ -22,7 +22,7 @@ class TargetRepository(object):
 
     def update(self):
         os.chdir(self.repository_path)
-        subprocess.check_output(['hg', 'update'])
+        subprocess.check_output(['hg', 'pull', '-u'])
 
 
 class TargetFileRevision(object):
```

(\*1) だったら hgweb に patch 投げればいいやん、と思って、[投げておきました](/blog/2015/12/29/patch-fixed-invalid-atom-log-feed-url-in-file-log-page/) ...
