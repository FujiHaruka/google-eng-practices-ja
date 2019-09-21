# 小さな CL

## どうして小さな CL を書くのか？ {#why}

小さくシンプルな CL には以下のような利点があります。

- **速くレビューできる。**レビュアーにとって、小さな CL をレビューするための 5 分間を数回確保するほうが、一度の大きな CL をレビューするための 30 分をまとめて確保するよりも容易です。
- **隅々までレビューできる。**変更が大規模だとあっちこっちに詳細なコメントを大量に書かねばならず、レビュアーと作成者がストレスを感じやすくなります。ときには重要な点を見落としたり省略したりしてしまうこともあります。
- **バグが混入する可能性が減る。**変更箇所が少なければ、開発者もレビュアーも CL の影響範囲が予測しやすく、バグが混入したかどうかを見分けやすくなります。
- **CL が却下されても無駄になる作業が少ない。**巨大な CL を書いてからレビュアーに全体的な方向性が間違っていると言われると、多くの作業が無駄になってしまいます。

- **マージしやすい。**大規模な CL での作業には時間がかかるため、マージする頃には多くのコンフリクトが発生し、頻繁にマージしなければならなくなります。
- **設計を改善しやすくなる。**小さな変更の設計やコードの健康状態を改善するほうが、大きな変更の詳細をすべて見直して設計を洗練させるよりもずっと簡単です。
- **レビューによって作業がブロックされなくなる。**あなたがしようとしている変更全体のうち一部を自己完結的な CL にして提出すれば、現在の CL のレビューを待つ間にコーディング作業を続けることができます。
- **ロールバックしやすい。**大きな CL は多くのファイルにわたって変更するため、最初に CL を提出してからロールバックの CL までにそれらのファイルが変更され、ロールバックが複雑になります。（その CL 以降の CL もロールバックする必要が生じることもあります）

注意していただきたいのですが、**レビュアーにはあなたの変更が大きすぎるというそれだけの理由でただちにその変更を却下できる裁量があります。**普通はそこまでせず、レビュアーはあなたの貢献に感謝しつつも、もっと小さな変更に分けて CL を送ってほしいとリクエストするでしょう。すでにコードを書いたあとで変更を分割するのは骨の折れる作業になりますし、あるいは巨大な変更を受け入れてもらうにしてもレビュアーが納得できるよう議論を交わすのにも多くの時間が必要になります。最初から小さな CL を書くほうが簡単です。

## 「小さい」とはどういうことか？ {#what_is_small}

一般に、CL の適切なサイズは**単一の自己完結的な変更**です。これは以下のことを意味します。

- CL が**ただ一つのこと**に取り組むミニマルな変更を行っています。これは普通、ある機能の全体を一度に実装するというより、その中の一部分だけを実装する CL です。小さすぎる CL を書いて失敗するほうが大きすぎる CL を書いて失敗するよりもよほど良いです。レビュアーと協力してどんなサイズの CL なら受け入れられるかを探ってみてください。
- レビュアーがその CL について理解する必要のあるすべての情報 (将来の開発を除く) が CLや、CL ディスクリプション、既存のコードベース、これまでレビューした CL の中にあります。
- システムはその CL がチェックインされたあともユーザーにとっても開発者にとっても正常に動作し続けます。
- CL がその含意がわかりにくくなるほどに過剰に小さくはありません。新しい API を追加するのなら、同じ CL の中にその API の使い方を含めるべきです。それはレビュアーが API の使い方をきちんと理解できるようになるためです。こうすることは、使われていない API のチェックインを防止します。

どれほど大きければ「大きすぎる」と言えるのかを判断する厳密で手っ取り早いルールはありません。大雑把には 100 行の CL は適度なサイズで、1000 行になると大きすぎると言えますが、これもレビュアーの判断次第です。変更するファイル数も CL の「サイズ」に関係あります。200 行の変更が行われていてもそれが 1 つのファイルで完結していれば許容できるかもしれませんが、 50 ファイルにもわたる変更であれば普通は「大きすぎる」と判断されるでしょう。

覚えておいていただきたいのですが、あなたはコードを書き始めた瞬間からコードに没頭していて目を閉じてもコードを思い浮かべられるとしても、レビュアーは何のコンテキストも持たないことがよくあります。あなたにとって許容範囲な CL のサイズのように思えても、レビュアーにとっては大きすぎるかもしれません。CL が小さすぎるからといってそのことで不満をこぼすレビュアーはめったにいません。

## どのようなときに大きな CL でも良いか？ {#large_okay}

大きな変更でも許される状況が例外的にあります。

- 一つのファイルをまるごと削除する場合は、一行だけの変更とみなしても構いません。レビューするのにそれほど長い時間がかからないからです。
- 大規模な CL がリファクタリングツールによって自動生成される場合があります。そのツールをあなたが完全に信頼していれば、そういった CL が大きくても構いません。レビュアーの仕事は正常に動作していることを確認して、意図通りに変更されていると言うだけです。このような CL は大きくなっても構いません。ただし、上で述べた注意書き（マージやテストなど）はこの場合にも適用されます。

### ファイルによる分割 {#splitting-files}

CL を分割する他のやり方として、別々のレビュアーを必要とするという点を除けば自己完結的な変更となる複数のファイルをひとまとめにグループ分けするというのがあります。

例1: ある protocol buffer を修正する CL を一つ送り、その proto を使用するコードを変更する CL を別で送ります。proto 変更の CL をコード変更の CL よりも先に提出する必要がありますが、二つの CL は同時にレビューを受けられます。この場合、両レビュアーに変更のコンテキストを提供するため他方の CL に関する情報を知らせてもよいでしょう。

例2: コード変更の CL を一つ送り、そのコードを使用する構成や実験のために CL を別で送ります。このやり方では必要ならロールバックも容易にできます。構成・実験用のファイルはコード変更よりも素早くプロダクションにプッシュされることがあるからです。

## リファクタリングを分離する {#refactoring}

It's usually best to do refactorings in a separate CL from feature changes or
bug fixes. For example, moving and renaming a class should be in a different CL
from fixing a bug in that class. It is much easier for reviewers to understand
the changes introduced by each CL when they are separate.

Small cleanups such as fixing a local variable name can be included inside of a
feature change or bug fix CL, though. It's up to the judgment of developers and
reviewers to decide when a refactoring is so large that it will make the review
more difficult if included in your current CL.

## Keep related test code in the same CL {#test_code}

Avoid splitting test code into a separate CL. Tests validating your code
modifications should go into the same CL, even if it increases the code line
count.

However, <i>independent</i> test modifications can go into separate CLs first,
similar to the [refactorings guidelines](#refactoring). That includes:

- validating pre-existing, submitted code with new tests.
- refactoring the test code (e.g. introduce helper functions).
- introducing larger test framework code (e.g. an integration test).

## Don't Break the Build {#break}

If you have several CLs that depend on each other, you need to find a way to
make sure the whole system keeps working after each CL is submitted. Otherwise
you might break the build for all your fellow developers for a few minutes
between your CL submissions (or even longer if something goes wrong unexpectedly
with your later CL submissions).

## Can't Make it Small Enough {#cant}

Sometimes you will encounter situations where it seems like your CL *has* to be
large. This is very rarely true. Authors who practice writing small CLs can
almost always find a way to decompose functionality into a series of small
changes.

Before writing a large CL, consider whether preceding it with a refactoring-only
CL could pave the way for a cleaner implementation. Talk to your teammates and
see if anybody has thoughts on how to implement the functionality in small CLs
instead.

If all of these options fail (which should be extremely rare) then get consent
from your reviewers in advance to review a large CL, so they are warned about
what is coming. In this situation, expect to be going through the review process
for a long time, be vigilant about not introducing bugs, and be extra diligent
about writing tests.

Next: [How to Handle Reviewer Comments](handling-comments.md)
