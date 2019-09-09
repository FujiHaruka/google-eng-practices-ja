# レビューで CL を閲覧する

## 要約

前節で[コードレビューの観点](looking-for.md)を学びましたが、では複数ファイルにまたがった変更をレビューする最も効果的な方法は何でしょうか？

1. 変更はうまく機能するでしょうか？適切な[ディスクリプション](../developer/cl-descriptions.md)はあるでしょうか？
2. いちばん重要な変更を最初に確認してください。それは全体としてうまく設計されているでしょうか？
3. CL の残りの部分を適切な順序で見てください。

## ステップ 1: 変更を広く眺める {#step_one}

[CL のディスクリプション](../developer/cl-descriptions.md)と CL が大まかに何をしているかを確認してください。
この変更は機能しているでしょうか？
そもそもこの変更がしてははならないものであれば、変更すべきでない理由を添えてすぐに返信してください。
そのように変更を却下する場合、代わりに何をすべきかを開発者に提案するのも良い考えです。

たとえば、次のように伝えることができます。「これに関して良い仕事をしてくださっているように思えます。ありがとう！ですが、実はあなたがここで修正した FooWidget システムは削除する方向で進めているため、今のところ新しい修正をしたくないのです。代わりに新しい BarWidget クラスをリファクタリングしていただくのはどうでしょうか？」

注意していただきたいのですが、レビュアーは現在の CL を却下して代わりの提案をしただけではなくて、それを礼儀正しく伝えました。
このような礼儀正しさは重要です。私達は意見が一致しないときでも開発者として互いを尊重し合うということを示したいからです。

変更してほしくない部分を変更する CL がいくつも送られる場合、
チームの開発プロセスや外部のコントリビューター向けに投稿されたプロセスを再検討してください。
CL が書かれる前にもっとコミュニケーションが必要です。
人々が 1 トンもの仕事をしてその後で破棄や大部分の書き直しを迫られるよりは、事前に「ノー」と言えるほうが良いでしょう。

## Step Two: Examine the main parts of the CL {#step_two}

Find the file or files that are the "main" part of this CL. Often, there is one
file that has the largest number of logical changes, and it's the major piece of
the CL. Look at these major parts first. This helps give context to all of the
smaller parts of the CL, and generally accelerates doing the code review. If the
CL is too large for you to figure out which parts are the major parts, ask the
developer what you should look at first, or ask them to
[split up the CL into multiple CLs](../developer/small-cls.md).

If you see some major design problems with this part of the CL, you should send
those comments immediately, even if you don't have time to review the rest of
the CL right now. In fact, reviewing the rest of the CL might be a waste of
time, because if the design problems are significant enough, a lot of the other
code under review is going to disappear and not matter anyway.

There are two major reasons it's so important to send these major design
comments out immediately:

- Developers often mail a CL and then immediately start new work based on that
  CL while they wait for review. If there are major design problems in the CL
  you're reviewing, they're also going to have to re-work their later CL. You
  want to catch them before they've done too much extra work on top of the
  problematic design.
- Major design changes take longer to do than small changes. Developers nearly
  all have deadlines; in order to make those deadlines and still have quality
  code in the codebase, the developer needs to start on any major re-work of
  the CL as soon as possible.

## Step Three: Look through the rest of the CL in an appropriate sequence {#step_three}

Once you've confirmed there are no major design problems with the CL as a whole,
try to figure out a logical sequence to look through the files while also making
sure you don't miss reviewing any file. Usually after you've looked through the
major files, it's simplest to just go through each file in the order that
the code review tool presents them to you. Sometimes it's also helpful to read the tests
first before you read the main code, because then you have an idea of what the
change is supposed to be doing.

Next: [Speed of Code Reviews](speed.md)
