# コードレビューの観点

（注）以下のポイントを検討する際にはつねに[コードレビューの基準](standard.md)を忘れないでください。

## 設計

レビューで確認すべき最も大切なことは、CL の全体的な設計です。
CL のコードの各部分は相互にきちんと連携するでしょうか？この変更はコードベースに属するものでしょうか、それともあるライブラリに属するものでしょうか？システムの他の部分とうまく統合するでしょうか？この機能を追加するタイミングは今がふさわしいでしょうか？

## 機能性

この CL は開発者の意図通りに動作しますか？開発者の意図はこのコードのユーザーにとって良いものでしょうか？「ユーザー」とは普通、エンドユーザー（その変更によって影響を受ける場合）と開発者（将来このコードを「使う」必要のある人）の両方を指します。

通常、CL がコードレビューに至るまでには、コードが正しく動作することを開発者が十分にテストしていると期待できます。
それでもレビュアーはエッジケースを想定する、並行処理の問題を探す、ユーザーになりきって考えるなど、コードを読むだけではわからない不具合がないかを確認してください。

必要に応じて CL を検証**しても構いません**。特に、**UI の変更**といった、ユーザーに影響する変更があるときは、レビュアーが CL の動作を確認するべき最も重要な機会です。
変更がユーザーにどのような影響を与えるかはコードを読むだけではわかりにくいものです。
そのような変更に対しては、CL の変更を反映して自分で動作確認するのが大変なら、開発者にその機能のデモを依頼することもできます。

また別のケースですが、コードレビュー中に機能についてしっかり考えることが特に重要なのは、CL にある種の**並行プログラミング**が行われていて、理論的にデッドロックや競合状態を引き起こす可能性がある場合です。
こういう問題はコードを実行するだけでは不具合の発見が難しいため、通常、コード全体を見て問題が発生していないことを注意深く確認する人（開発者とレビュアーの両方）が必要です。
（なお、これこそがやはり、競合状態やデッドロックが発生しうるところで並行処理モデルを採用すべきでない十分な理由です。コードレビューを行うのもコードを理解するのも非常に複雑になりうるからです。）

## 複雑性

CL が必要以上に複雑になっていないでしょうか？ CL のあらゆるレベルで確認してください。
一行一行は複雑すぎないでしょうか？関数は複雑すぎないでしょうか？クラスは複雑すぎないでしょうか？
「複雑すぎる」とは通常、**「コードを読んですぐに理解できない」**という意味です。
あるいは、**「開発者がこのコードを呼び出したり修正したりしようとするときに不具合を生み出しそうである」**という意味でもあります。

あるタイプの複雑性は、**オーバーエンジニアリング**です。開発者が必要以上にコードを一般化していたり、現在のシステムにとってまだ必要のない機能を盛り込んでいたりするということです。
レビュアーはオーバーエンジニアリングを特に警戒すべきです。
開発者には、**今**解決する必要のある既知の問題に取り組むべきであって、将来解決する必要が出てくる**かもしれない**推測に基づいた問題には目を向けないよう勧めてください。
将来の問題はそれが発生してから取り組めばよいのです。問題が発生すれば、この物理的な宇宙の中でその実際の形と要件を知ることができます。

## Tests

Ask for unit, integration, or end-to-end
tests as appropriate for the change. In general, tests should be added in the
same CL as the production code unless the CL is handling an
[emergency](../emergencies.md).

Make sure that the tests in the CL are correct, sensible, and useful. Tests do
not test themselves, and we rarely write tests for our tests—a human must ensure
that tests are valid.

Will the tests actually fail when the code is broken? If the code changes
beneath them, will they start producing false positives? Does each test make
simple and useful assertions? Are the tests separated appropriately between
different test methods?

Remember that tests are also code that has to be maintained. Don't accept
complexity in tests just because they aren't part of the main binary.

## Naming

Did the developer pick good names for everything? A good name is long enough to
fully communicate what the item is or does, without being so long that it
becomes hard to read.

## Comments

Did the developer write clear comments in understandable English? Are all of the
comments actually necessary? Usually comments are useful when they **explain
why** some code exists, and should not be explaining *what* some code is doing.
If the code isn't clear enough to explain itself, then the code should be made
simpler. There are some exceptions (regular expressions and complex algorithms
often benefit greatly from comments that explain what they're doing, for
example) but mostly comments are for information that the code itself can't
possibly contain, like the reasoning behind a decision.

It can also be helpful to look at comments that were there before this CL. Maybe
there is a TODO that can be removed now, a comment advising against this change
being made, etc.

Note that comments are different from *documentation* of classes, modules, or
functions, which should instead express the purpose of a piece of code, how it
should be used, and how it behaves when used.

## Style

We have [style guides](http://google.github.io/styleguide/) at Google for all
of our major languages, and even for most of the minor languages. Make sure the
CL follows the appropriate style guides.

If you want to improve some style point that isn't in the style guide, prefix
your comment with "Nit:" to let the developer know that it's a nitpick that you
think would improve the code but isn't mandatory. Don't block CLs from being
submitted based only on personal style preferences.

The author of the CL should not include major style changes combined with other
changes. It makes it hard to see what is being changed in the CL, makes merges
and rollbacks more complex, and causes other problems. For example, if the
author wants to reformat the whole file, have them send you just the
reformatting to as one CL, and then send another CL with their functional
changes after that.

## Documentation

If a CL changes how users build, test, interact with, or release code, check to
see that it also updates associated documentation, including
READMEs, g3doc pages, and any generated
reference docs. If the CL deletes or deprecates code, consider whether the
documentation should also be deleted.
If documentation is
missing, ask for it.

## Every Line {#every_line}

Look at *every* line of code that you have been assigned to review. Some things
like data files, generated code, or large data structures you can scan over
sometimes, but don't scan over a human-written class, function, or block of code
and assume that what's inside of it is okay. Obviously some code deserves more
careful scrutiny than other code—that's a judgment call that you have to
make—but you should at least be sure that you *understand* what all the
code is doing.

If it's too hard for you to read the code and this is slowing down the review,
then you should let the developer know that
and wait for them to clarify it before you try to review it. At Google, we hire
great software engineers, and you are one of them. If you can't understand the
code, it's very likely that other developers won't either. So you're also
helping future developers understand this code, when you ask the developer to
clarify it.

If you understand the code but you don't feel qualified to do some part of the
review, make sure there is a reviewer on the CL who is qualified, particularly
for complex issues such as security, concurrency, accessibility,
internationalization, etc.

## Context

It is often helpful to look at the CL in a broad context. Usually the code
review tool will only show you a few lines of code around the parts that are
being changed. Sometimes you have to look at the whole file to be sure that the
change actually makes sense. For example, you might see only four new lines
being added, but when you look at the whole file, you see those four lines are
in a 50-line method that now really needs to be broken up into smaller methods.

It's also useful to think about the CL in the context of the system as a whole.
Is this CL improving the code health of the system or is it making the whole
system more complex, less tested, etc.? **Don't accept CLs that degrade the code
health of the system.** Most systems become complex through many small changes
that add up, so it's important to prevent even small complexities in new
changes.

## Good Things {#good_things}

If you see something nice in the CL, tell the developer, especially when they
addressed one of your comments in a great way. Code reviews often just focus on
mistakes, but they should offer encouragement and appreciation for good
practices, as well. It’s sometimes even more valuable, in terms of mentoring, to
tell a developer what they did right than to tell them what they did wrong.

## Summary

In doing a code review, you should make sure that:

- The code is well-designed.
- The functionality is good for the users of the code.
- Any UI changes are sensible and look good.
- Any parallel programming is done safely.
- The code isn't more complex than it needs to be.
- The developer isn't implementing things they *might* need in the future but
  don't know they need now.
- Code has appropriate unit tests.
- Tests are well-designed.
- The developer used clear names for everything.
- Comments are clear and useful, and mostly explain *why* instead of *what*.
- Code is appropriately documented (generally in g3doc).
- The code conforms to our style guides.

Make sure to review **every line** of code you've been asked to review, look at
the **context**, make sure you're **improving code health**, and compliment
developers on **good things** that they do.

Next: [Navigating a CL in Review](navigate.md)
