# コードレビューのスピード

## なぜコードレビューは素早く行うべきか？ {#why}

**Google では開発者のチームが協力してプロダクトを高速に開発するために最適化しており**、開発者個人がコードを高速に書くための最適化はしません。
開発者個人のスピードは確かに重要ですが、チーム全体のスピードと比べると**同等の**重要性があるわけではありません。

コードレビューが遅滞するとさまざまなことが起こります。

- **チーム全体の開発速度が減少します。**もちろん、レビューに素早く反応しなくても個人としては他の仕事を終わらせられます。
  けれども、チームの他のメンバーが書いた新機能や不具合修正は、CL がレビュー待ち、再レビュー待ちになると何日も何週間も送れることになります。
- **開発者がコードレビューのプロセスに不満を持ち始めます。**
  レビュアーが数日おきにしか返信しないのに毎回 CL への大きな変更が要求されると、開発者はストレスをためるし開発が困難になります。
  よくあることですが、このようなときに表現される不満は、レビュアーが「厳しすぎる」というものです。
  本質的には**同じ**変更（実際にコードの健康状態を良くする変更）でも開発者の更新に応じてレビュアーが毎回**素早く**返信すれば、このような不満は薄れます。
  **コードレビューに関する不満はたいていの場合、プロセスをテンポ良く進めるだけで解消します。**
- **コードの健康状態が悪い影響を受けます。**
  レビューが遅いと、最高の出来映えといえないような CL でもとにかく提出してしまってよいという雰囲気が開発者の間に広がります。
  また、レビューの遅れはコードをきれいにしたり、リファクタリングしたり、既存の CL をさらに改善したりする意欲をそぎます。

## コードレビューはどれほど急ぐべきか？ {#fast}

あるタスクに集中的に取り組んでいる最中でなければ、**コードレビューの依頼が来たらすぐに着手してください。**

コードレビューのリクエストに返信するまでの**最長の時間は一営業日**です（つまり遅くとも翌朝一番に返信すべきです）。

このガイドラインに従うと、典型的な CL は（必要なら）一日以内に複数ラウンドに渡ってレビューが行われることになります。

## スピード vs 割り込み {#interruption}

チームの速度よりも個人の速度を尊重したほうが効率が良い場合があります。
**コードを書くような集中的に取り組むべきタスク**の最中には、自分のタスクを中断してコードレビューしてはいけません。研究結果によれば、開発者が割り込み作業のあとでスムーズな開発フローに戻るには長い時間がかかることがあります。
そのため、コーディングの最中に中断すると、他の開発者がコードレビューを多少待つよりも結果的に**余計な**コストがかかります。

それよりは仕事のブレークポイントを待ってからレビューのリクエストに返信しましょう。
たとえば今のコーディングが完了したときや、ランチの後や、ミーティングから帰ったとき、マイクロキッチンから戻ったときなどです。

## 素早い応答 {#responses}

コードレビューのスピードについて話すとき、私達の関心は**応答**時間の長さであり、レビュー全体が完了して提出されるまでの時間の長さではありません。
理想的にはプロセス全体も短時間で行うべきですが、** _個々の応答_ を素早く行うことのほうが、プロセス全体を短時間で終えることよりも遥かに重要です。**

たとえレビュー全体の**プロセス**に長い時間がかかってもレビュアーから素早く応答がもらえていれば、開発者が「遅い」コードレビューに感じる不満を大きく軽減できます。

あなたが非常に忙しくて CL のレビュー依頼をされても十分な時間が取れない場合、それでも素早く応答することはできます。いつ着手できるかを開発者に伝えたり、もっと短時間で応答できる他のレビュアーを紹介したり、[広い観点で見た初期段階のコメントを残したり](navigate.md)できます。（注：そういった返信をするとしてもコーディングを中断するべきではありません—仕事中にちょうどいいブレークポイントを見つけて返信してください。）

**レビュアーが十分な時間を取って、「LGTM」が個人の感想でなく「このコードは[私達の基準](standard.md)を満たしている」という意味だと言えるくらいにレビューするのは大切なことです。**同時に、理想的には個々の応答は[素早く](#fast)返信すべきです。

## Cross-Time-Zone Reviews {#tz}

When dealing with time zone differences, try to get back to the author when they
are still in the office. If they have already gone home, then try to make sure
your review is done before they get back to the office the next day.

## LGTM With Comments {#lgtm-with-comments}

In order to speed up code reviews, there are certain situations in which a
reviewer should give LGTM/Approval even though they are also leaving unresolved
comments on the CL. This is done when either:

- The reviewer is confident that the developer will appropriately address all
  the reviewer's remaining comments.
- The remaining changes are minor and don't _have_ to be done by the
  developer.

The reviewer should specify which of these options they intend, if it is not
otherwise clear.

LGTM With Comments is especially worth considering when the developer and
reviewer are in different time zones and otherwise the developer would be
waiting for a whole day just to get "LGTM, Approval."

## Large CLs {#large}

If somebody sends you a code review that is so large you're not sure when you
will be able to have time to review it, your typical response should be to ask
the developer to
[split the CL into several smaller CLs](../developer/small-cls.md) that build on
each other, instead of one huge CL that has to be reviewed all at once. This is
usually possible and very helpful to reviewers, even if it takes additional work
from the developer.

If a CL *can't* be broken up into smaller CLs, and you don't have time to review
the entire thing quickly, then at least write some comments on the overall
design of the CL and send it back to the developer for improvement. One of your
goals as a reviewer should be to always unblock the developer or enable them to
take some sort of further action quickly, without sacrificing code health to do
so.

## Code Review Improvements Over Time {#time}

If you follow these guidelines and you are strict with your code reviews, you
should find that the entire code review process tends to go faster and faster
over time. Developers learn what is required for healthy code, and send you CLs
that are great from the start, requiring less and less review time. Reviewers
learn to respond quickly and not add unnecessary latency into the review
process.
But **don't compromise on
the [code review standards](standard.md) or quality for an imagined improvement
in velocity**—it's not actually going to make anything happen more
quickly, in the long run.

## Emergencies

There are also [emergencies](../emergencies.md) where CLs must pass through the
_whole_ review process very quickly, and where the quality guidelines would be
relaxed. However, please see [What Is An Emergency?](../emergencies.md#what) for
a description of which situations actually qualify as emergencies and which
don't.

Next: [How to Write Code Review Comments](comments.md)
