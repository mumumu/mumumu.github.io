---
layout: post
title: "restore github page from source branch"
date: 2015-04-26 07:17
comments: true
categories: [tips]
---

github page を [Octopress](http://octopress.org/) で公開している人は、きっと master が HTML と CSS だけで出来たリポジトリを github に持っており、かつ source ブランチでそれを生成するための Rakefile や `_config.yml`、ブログエントリの markdown、テーマなどをバックアップしているはずだ。

では、何かの拍子にブログを書く環境を壊してしまい、source ブランチから復活させなければならなくなった場合はどうだろうか。今朝ちょうどそういう状況に陥ってしまい、復旧に少し手間取ったのでメモしておく。

要するに clone した後、source ブランチに移動し、 `_deploy` ディレクトリを生成する。そこで git リポジトリを再初期化し、github page へのリポジトリを remote に加えるだけだ。 要するに、 [setup_github_pagesタスクの後半](https://github.com/imathis/octopress/blob/5080107cb9e4c7bad8feb719f7e57c1da3b20c65/Rakefile#L352) を真似ただけである。

```
$ git clone git@github.com:mumumu/mumumu.github.io.git blog
$ cd blog
$ git checkout -b source origin/source
$ gem install bundler
$ bundle install
$ mkdir _deploy
$ cd _deploy
$ git init
$ git remote add origin git@github.com:mumumu/mumumu.github.io
```

あとは `rake new_post["some title"]` で記事を書き、`rake generate` や `rake gen_deploy` の操作でいつも通り記事が公開できるようになる。

こうしたリストアの操作と、 `git push origin source` というバックアップ操作が面倒くさかったので、以下のような patch を Rakefile に足しておいた。

まずは バックアップの操作が `rake gen_deploy`後に自動で行われるようにした。

```
@@ -252,16 +252,38 @@ multitask :push do
   Rake::Task[:copydot].invoke(public_dir, deploy_dir)
   puts "\n## copying #{public_dir} to #{deploy_dir}"
   cp_r "#{public_dir}/.", deploy_dir
+  message = "Site updated at #{Time.now.utc}"
   cd "#{deploy_dir}" do
     system "git add ."
     system "git add -u"
     puts "\n## Commiting: Site updated at #{Time.now.utc}"
-    message = "Site updated at #{Time.now.utc}"
     system "git commit -m \"#{message}\""
     puts "\n## Pushing generated #{deploy_dir} website"
     system "git push origin #{deploy_branch} --force"
+  end
+
+  cd "#{deploy_dir}/../source" do
+    system "git add *"
+    puts "\n## Commiting: Site updated at #{Time.now.utc}"
+    system "git commit -m \"#{message}\""
+    puts "\n## Pushing source branch as backup"
+    system "git push origin source"
     puts "\n## Github Pages deploy complete"
   end
+
+end
```

github page のソースを clone した後、`_deploy` ディレクトリを再生成し、master として remote を足すタスクも追加した。

```
+
+desc "restore github pages directory"
+task :restore_github_pages_directory do
+  puts "\n## Re-creating deploy directory"
+  rm_rf "#{deploy_dir}"
+  mkdir_p "#{deploy_dir}"
+
+  cd "#{deploy_dir}" do
+    repo_url = "git@github.com:mumumu/mumumu.github.io"
+    system "git init"
+    system "git remote add origin #{repo_url}"
+  end
+ end
 
 desc "Update configurations to support publishing to root or sub directory"
```

**[Update September 30th 2018 13:38 JST by m]**

Ruby 2.5.1 に環境をアップデートしたことをきっかけに、少し手順を追加しました。
