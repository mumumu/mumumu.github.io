---
layout: post
title: "SQLAlchemy - When do I construct a Session, commit, and close it?"
date: 2013-12-30 05:35
comments: true
categories: [python,sqlalchemy]
---

以下は、[SQLAlchemy](http://www.sqlalchemy.org/) の [When do I construct a Session, when do I commit it, and when do I close it?](http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html#when-do-i-construct-a-session-when-do-i-commit-it-and-when-do-i-close-it) の全訳である。この文章はデータベースへのクエリを発行するのに使う [Sessionクラス](http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html#sqlalchemy.orm.session.Session) を原則的にどう扱うべきかを説明しているが、途中でスコープという概念を持ち込んで説明しているので異常に英語がわかりづらくなっている。

さらに、[scoped\_session](http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html#sqlalchemy.orm.scoping.scoped_session) というヘルパ関数も持ち込んでいるので、一見しただけだと [scoped\_session](http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html#sqlalchemy.orm.scoping.scoped_session) がなんのためにあるのか非常にわかりづらくなっている。こうしたことから、全訳してメモとして残しておく。

### まとめ

**・** SQLAlchemy における Session の寿命は、データベースのデータにアクセスしたり管理したりするオブジェクトや関数と切り離し、それらの外部で扱うのが一般的なルール   
**・** Webアプリケーションの場合は、リクエストを処理し始めるときに Session のインスタンスを作り、終わるときに破棄すればよい。通常はフレームワークにそうしたフックが組み込まれているし、それを使うことを推奨   
**・** 上記のような機構がない場合に、[scoped\_session](http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html#sqlalchemy.orm.scoping.scoped_session) と呼ばれるヘルパがある。

こうした内容に興味がある人は、以下の全訳を読むとよいと思う。
<br />
----
</br />
### Sessionクラス のインスタンスを作ったり、commit や close メソッドを発行するタイミングはいつ？

<pre style="font-size:17px">

要約：

Session の寿命は、データベースのデータにアクセスしたり管理したりするオブジェクトや
関数と 「切り離し、そうした関数やオブジェクトの外部で」 扱うのが一般的なルールです。

</pre>
Session クラスはデータベースへのアクセスがあると思われる操作のはじめに作られるのが一般的です。

Session クラスは、 通信を始めるとすぐにトランザクションを開始します。 'autocommit' フラグを推奨されているデフォルトの 'False' にしたままだと、トランザクションは Session クラスの commit や、 rollback、そして close メソッドが実行されるまで続きます。 Session は再利用されると、以前のトランザクションが終了した後で新しいトランザクションを開始するでしょう。このことから、Session クラスは多くのトランザクションにまたがった寿命を持っているということになります。これらの二つの概念を **トランザクションスコープ** と **セッションスコープ** と呼ぶことにします。

ここでわかるのは、SQLAlchemy は開発者がアプリケーションの中でこれらふたつのスコープをはっきりさせておくことを推奨しているということです。これは、いつスコープが始まり、終了するかということだけでなく、これらのスコープが適用される範囲も含みます。たとえば、Session クラスのインスタンスは特定の関数やメソッド内の実行フローに対してはローカルであるべきですが、アプリケーション全体で使われるものについてはグローバルであるべきですし、これらのふたつの中間にあるインスタンスもあってよいはずです。

このスコープを決めることは、開発者にとって重荷ですし、SQLAlchemy が必然的にデータベースの扱い方に強い影響力を持つ領域です。 変更を集めて一定時間ごとにフラッシュし、メモリの状態をローカルのトランザクションと同期する [unit of work](http://docs.sqlalchemy.org/en/rel_0_8/glossary.html#term-unit-of-work) パターンがあります。このパターンは意味のあるトランザクションスコープが存在する場合にだけ有用です。

アプリケーションによって状況は様々ですが、最良な Sessionクラス のスコープを決めるのは通常はあまり難しくありません。

Sessionクラス のインスタンスをトランザクションが終わると同時に破棄する方法がよくとられます。これはトランザクションスコープとセッションスコープが同じであることを意味しています。これは出発点としては良いやり方です。なぜなら、セッションスコープとトランザクションスコープを別に考える必要がないからです。

トランザクションスコープを決めるやり方については万能なものはありませんが、一般的なパターンはあります。特に Webアプリケーションを書いているなら、選択肢は決まっています。

Webアプリケーション は単一の一貫したスコープ - **リクエスト** - の範囲でアプリケーションが既に構築されているため、一番簡単です。このスコープはブラウザからリクエストを受け付け、レスポンスを作るためにそれを処理し、最後にブラウザにレスポンスを返すことまでを表しています。Webアプリケーションを Sessionクラス と統合するのは Sessionクラス のスコープをこのリクエストに結びつけるだけの素直なタスクです。 Session はリクエストがはじまるときに作ることもできますし、必要になったときに lazy initializationパターン を使って作ることもできます。リクエストの処理はさらに続き、アプリケーションロジックが実際のリクエストオブジェクトにアクセスするやり方と似たやり方で Session にアクセスできるシステムもあります。リクエストが終了すると、通常は Webフレームワークが提供しているイベントフックを使って Session も破棄されます。 Session によって使われたトランザクションもこの時点でコミットされます。アプリケーションが明示的にコミットするオプションもありますし、アプリケーションが保証した場合にだけコミットする場合もありますが、想定外のことが起きてリクエストが終了する際に破棄される可能性も常にあります。

ほとんどの Webアプリケーションフレームワークは Session クラスを リクエスト と結びつける基盤を確立しており、Sessionオブジェクト が適切にインスタンス化され、リクエストの終了時に破棄されるようになっています。こうした基盤は Flask と組み合わせて SQLAlchemy を使う場合は [Flask-SQLAlchemy](http://pythonhosted.org/Flask-SQLAlchemy/) に、 [Pyramid](http://www.pylonsproject.org/) や [Zope framework](http://www.zope.org/) と組み合わせて使う場合、[Zope-SQLAlchemy](https://pypi.python.org/pypi/zope.sqlalchemy) のようなプロダクトに含まれています。SQLAlchemy はこれらのプロダクトをできる限り使うことを強く推奨しています。

上記のようなライブラリが利用できない場合に、SQLAlchemy は [scoped\_session](http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html#sqlalchemy.orm.scoping.scoped_session) というヘルパークラスを用意しています。このオブジェクトの使い方に関するチュートリアルが [Contextual/Thread-local Sessions](http://docs.sqlalchemy.org/en/rel_0_8/orm/session.html#contextual-thread-local-sessions) のページにあります。このチュートリアルでは、Session を現在のスレッドに関連づける簡単な方法や、Sessionオブジェクト を他のスコープに関連づけるパターンも示しています。

既に述べたとおり、Webアプリケーション以外では、アーキテクチャのパターンがひとつではないために明快なパターンはありません。一番良い戦略は、"オペレーション(操作)" の境界を決め、特定のスレッドが一連の操作をはじめるときに着目し、それが終わるときに commit メソッドを実行することです。以下に例を示します:

**・** 子プロセス を fork するバックグラウンドの デーモン は、 Session を子プロセスにローカルなものとして生成し、その子プロセスが実行するジョブが実行されている間それを使い、ジョブが完了したらそれを破棄したいと考えるでしょう。

**・** コマンドラインスクリプトのために、アプリケーションはグローバルな Session をひとつ作るでしょう。これはプログラムが開始されるときに作られ、タスクが完了したらすぐに commit メソッドを実行します。

**・** GUI インターフェイスを持つアプリケーションでは、Session のスコープは「ボタンを押したとき」のような、ユーザーが生成するイベントの範囲にするのがベストかもしれません。もしくは、明示的なユーザーの操作、たとえば一連のレコードを"取得"し、それを"保存"するといったものに対応付けてもよいかもしれません。

一般的なルールとして、アプリケーションは Session の寿命を特定のデータを扱う関数とは **別に** 管理すべきです。これはデータに特有の操作と、データににアクセスし管理するコンテキストは基本的に別の関心事として扱うと言うことです。

E.g. **真似しないでね**

<pre style="font-size:15px">
    ### 以下は **間違ったやり方です** ###

    class ThingOne(object):
        def go(self):
            session = Session()
            try:
                session.query(FooBar).update({"x": 5})
                session.commit()
            except:
                session.rollback()
                raise

    class ThingTwo(object):
        def go(self):
            session = Session()
            try:
                session.query(Widget).update({"q": 18})
                session.commit()
            except:
                session.rollback()
                raise

    def run_my_program():
        ThingOne().go()
        ThingTwo().go()
</pre>

そうではなくて、Session (と、通常はトランザクション)の寿命は、**分離し、外側で扱いましょう**

<pre style="font-size:15px">
    ### 以下は **より良い** (ですが唯一ではない) やり方です ###

    class ThingOne(object):
        def go(self, session):
            session.query(FooBar).update({"x": 5})

    class ThingTwo(object):
        def go(self, session):
            session.query(Widget).update({"q": 18})

    def run_my_program():
        session = Session()
        try:
            ThingOne().go(session)
            ThingTwo().go(session)

            session.commit()
        except:
            session.rollback()
            raise
        finally:
            session.close()
</pre>

高度な開発者は、Session やトランザクションおよび例外のハンドリングを、プログラムがやっていることの詳細からできるだけ切り離そうとするでしょう。たとえば、[context manager](http://docs.python.org/3/library/contextlib.html#contextlib.contextmanager) を使い、これらをさらに分離することができます。

<pre style="font-size:15px">
    ### 別の (しかし *しつこいが唯一ではない*) やり方 ###

    from contextlib import contextmanager

    @contextmanager
    def session_scope():
        """Provide a transactional scope around a series of operations."""
        session = Session()
        try:
            yield session
            session.commit()
        except:
            session.rollback()
            raise
        finally:
            session.close()


    def run_my_program():
        with session_scope() as session:
            ThingOne().go(session)
            ThingTwo().go(session)
</pre>
