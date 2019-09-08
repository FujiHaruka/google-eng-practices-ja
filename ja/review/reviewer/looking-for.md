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

## テスト

変更に適したユニットテスト、結合テスト、E2E テストを依頼してください。
一般に、テストはプロダクションコードと同じ CL に追加してください。
例外は、CL が[緊急事態](../emergencies.md)に対処している場合です。

CL の中のテストが正確で、適切で、有用であることを確認してください。
テストがテストコード自体をテストすることはありませんし、テストのためのテストコードを書くこともめったにありません。テストの有効性は人間が確認しなければなりません。

コードが壊れているときにテストはきちんと失敗するでしょうか？
そのテストの下でコードを変更すると、テストが誤検知を起こさないでしょうか？
各テストはシンプルで有用なアサーションを使っているでしょうか？
テストは異なるテストメソッドごとに適切に分割されているでしょうか？

テストもまた保守すべきコードであることを覚えていてください。
メインのバイナリに含まれないからといって、テストが複雑になるのを許容しないでください。

## 命名

開発者はあらゆるものに適切な名前を与えているでしょうか？
適切な名前とは、それが何であるか／何をするかを伝えるのに十分に長く、しかし読むのに困難を覚えないほど短いものです。

## コメント

開発者はわかりやすい英語で明確なコメントを書きましたか？
すべてのコメントは実際に必要でしょうか？
コメントは普通、あるコードが**「なぜ」**存在するのかを説明するのに役立ちますが、コードが**「何」**をしているのかを説明すべきではありません。
コードがそれ自身を説明するほど明確でないのなら、コードをもっとシンプルにすべきです。
例外はあります（たとえば正規表現や複雑なアルゴリズムでは何をしているのかを説明するコメントは非常に有益です）が、ほとんどの場合、コメントは決定の背後にある理由のような、コードそのものが語ることのできない情報を伝えるために書きます。

CL の前にその箇所にあったコメントに注目するのが有益であることもあります。
今となっては削除すべき TODO があるかもしれませんし、この変更を行うべきではないと助言するコメントなどがあるかもしれません。

なお、コメントはクラス、モジュール、関数の**ドキュメンテーション**とは違います。ドキュメンテーションコメントはコードの目的や、使い方や、使われたときのふるまいを記述するものです。

## スタイル

Google には[スタイルガイド](http://google.github.io/styleguide/)があります。メジャーな言語に関してはすべて、マイナーな言語でも多くはスタイルガイドが揃っています。
CL が適切なスタイルガイドを従っているかを確認してください。

スタイルガイドに記載のないスタイルの改善をしたい場合、コメントに「Nit:」というプレフィックスを付けて、それが細かい指摘 (nitpick) であることを開発者に知らせるのがよいでしょう。そうすると、コードを改善してほしいが強制ではないということが伝わります。
個人的なスタイルの好みで CL の提出をブロックしないでください。

CL の作成者はスタイル上の大きな変更を他の変更に混ぜないようにしてください。
それをすると CL で何が変更されているのかを見るのが大変になり、マージ後にロールバックするのはもっと大変で、また他の問題も引き起こします。
たとえば、作成者がファイル全体を再フォーマットしたいと思ったら、再フォーマットするだけの変更を一つの  CL として提出し、その後で機能的な変更を別の CL として提出するようにしてください。

## ドキュメンテーション

CL がコードのビルド、テスト、相互作用、リリースのやり方を変更する場合、それに関連するドキュメンテーションも更新しているかを確認してください。
関連するドキュメンテーションには、README、g3doc ページ、自動生成されたリファレンスドキュメントなどがあります。
CL がコードを削除または非推奨にしたら、ドキュメンテーションもも削除するべきかどうか考えてください。
ドキュメンテーションがなければ、作成するように依頼してください。

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
