# コードレビューの基準

コードレビューの主な目的は、Google のコードベースが全体として健全であるようにし、また時間とともに改善することを保証することです。
コードレビューのすべてのツールとプロセスは、その目的のために設計されています。

これを実現するため、さまざまなトレードオフがバランスを取る必要があります。

まず、開発者は自分のタスクを**進める**ことができなければなりません。
コードベースに改善を提出できなければ、コードベースは改善しません。
また、レビュアーが**どんな**変更でもそれを取り入れるを非常に困難にしている場合、開発者は将来の改善を行う意欲を失います。

一方、CL の品質が、コードベース全体のコードの健全性を時間とともに減少させるようなものではないということを確認するのはレビュアーの責任です。
これは骨の折れるやっかいな仕事になることがあります。
というのも多くの場合、時間の経過とともにコードの健全性を小さく減少させる要因が重なり、コードベースの品質は下がっていきます。
特に、チームが無視できない時間的制約のもとにあり、チームがゴールを達成するためにショートカットをしなければならないと感じているときには、コードベースの品質は下がりやすいのです。

また、レビュアーにはレビューしているコードに対して所有権と責任があります。
レビュアーはコードベースの一貫性と保守可能性を維持し、[「コードレビューで求めるものは何か」](looking-for.md)で言及されている事柄が守られるように保証したいと考えています。

そのため、私達はコードレビューにおいて期待する基準として、次のルールを見出しました。

**一般に、CL が完璧でなくても、その変更が作業中のシステムの全体的なコードの健全性を間違いなく改善するとわかれば、レビュアーは CL を積極的に承認すべきである。**

これはすべてのコードレビューガイドラインの中で**最上位の**原則です。

もちろん、このルールに制限はあります。たとえば、CL がシステムにレビュアーが望んでいない機能を追加している場合、コードがうまく設計されてるとしてもレビュアーは承認を拒否することができます。

ここでのポイントは、「完璧な」コードといったものは存在しないということです—あるのは**より良い**コードだけです。
レビュアーは CL を承認する前にコードのすみずみまで磨きをかけることを作成者に要求すべきではありません。
むしろ、前に進めるべきかどうかの必要性を、CL 作成者が提案している変更の重要性と比較して、レビュアーはバランスよく検討すべきです。
レビュアーが追求すべきは、完璧さではなく**継続的な改善**です。
CL が全体としてシステムの保守性、可読性、理解可能性を向上させるのなら、それが「完璧」でないからといって数日も数週間も遅らせるべきではありません。

レビュアーは何か改善できそうな点を見つけたら、コメントを残すのに躊躇する必要はありません。
**いつでも**気軽にコメントを残すべきです。
その際に、もし重要でない指摘であれば、「Nit: 」（あら探しという意味）のようなプレフィックスを付けて、
CL 作成者にこれはただ磨きをかけるための指摘なので無視してもらっても構わない、と知らせると良いでしょう。

（注）このドキュメントは、システムのコード全体の健全性を**悪化**させるとわかりきっている CL を正当化しません。
唯一それが許されるケースは、[緊急事態](../emergencies.md)にあります。

## Mentoring

Code review can have an important function of teaching developers something new
about a language, a framework, or general software design principles. It's
always fine to leave comments that help a developer learn something new. Sharing
knowledge is part of improving the code health of a system over time. Just keep
in mind that if your comment is purely educational, but not critical to meeting
the standards described in this document, prefix it with "Nit: " or otherwise
indicate that it's not mandatory for the author to resolve it in this CL.

## Principles {#principles}

- Technical facts and data overrule opinions and personal preferences.

- On matters of style, the [style guide](http://google.github.io/styleguide/)
  is the absolute authority. Any purely style point (whitespace, etc.) that is
  not in the style guide is a matter of personal preference. The style should
  be consistent with what is there. If there is no previous style, accept the
  author's.

- **Aspects of software design are almost never a pure style issue or just a
  personal preference.** They are based on underlying principles and should be
  weighed on those principles, not simply by personal opinion. Sometimes there
  are a few valid options. If the author can demonstrate (either through data
  or based on solid engineering principles) that several approaches are
  equally valid, then the reviewer should accept the preference of the author.
  Otherwise the choice is dictated by standard principles of software design.

- If no other rule applies, then the reviewer may ask the author to be
  consistent with what is in the current codebase, as long as that doesn't
  worsen the overall code health of the system.

## Resolving Conflicts {#conflicts}

In any conflict on a code review, the first step should always be for the
developer and reviewer to try to come to consensus, based on the contents of
this document and the other documents in [The CL Author's Guide](../developer/)
and this [Reviewer Guide](index.md).

When coming to consensus becomes especially difficult, it can help to have a
face-to-face meeting or a VC between the reviewer and the author, instead of
just trying to resolve the conflict through code review comments. (If you do
this, though, make sure to record the results of the discussion in a comment on
the CL, for future readers.)

If that doesn't resolve the situation, the most common way to resolve it would
be to escalate. Often the
escalation path is to a broader team discussion, having a TL weigh in, asking
for a decision from a maintainer of the code, or asking an Eng Manager to help
out. **Don't let a CL sit around because the author and the reviewer can't come
to an agreement.**

Next: [What to look for in a code review](looking-for.md)
