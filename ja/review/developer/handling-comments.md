# レビューコメントの対応の仕方

CL をレビューに送ると、レビュアーが CL にいくつかのコメントを残してくれます。ここではレビュアーのコメントに対応するときに知っておくと有益なポイントを紹介します。

## 個人の人格へのコメントとして受け取らない {#personal}

レビューの目的は私達のコードベースと私達のプロダクトの品質を維持することです。レビュアーがあなたのコードに苦言を呈したら、それを個人攻撃とかあなたの能力をくさしているとか受け取らずに、レビュアーがあなたを手助けしようとしていると考えてみてください。レビュアーはコードベースを、また Google を良くしようとしているのです。

ときにはレビュアーが苛立って、そのイライラをコメントに表現することもあります。これはレビュアーとして褒められた行いではありませんが、開発者としてはこうしたことへの心構えをしましょう。ご自分にこう問いかけてください。「レビュアーが私に伝えようとしている建設的な事柄は何だろう？」と。そして、それがレビュアーの真意だと捉えてください。

**コードレビューコメントに対して怒りに任せて反応しないでください。**怒りに任せたコメントはプロとしての礼儀作法に違反しますし、それがコードレビューツールに永遠に残ることになります。もしあなたが怒りや苛立ちで丁重に応答できなくなっていれば、しばらく席を立って歩いたり他の作業に当たったりして、気持ちが落ち着いて丁重な応答ができるようになるのを待ってください。

一般に、レビュアーが建設的で礼儀正しい言い方でフィードバックをしてくれない場合、そのことを対面で伝えましょう。対面やビデオ通話で会話する機会が持てなければ、個人的なメールを送りましょう。レビュアーの言い方のどこが嫌でどんなふうに変えてほしいのかを丁重に説明してください。この個人的な会話でもレビュアーが非建設的な言い方で応酬するようなら、あるいは態度が全く変わらないようなら、上司に相談するのが適切です。

## Fix the Code {#code}

If a reviewer says that they don't understand something in your code, your first
response should be to clarify the code itself. If the code can't be clarified,
add a code comment that explains why the code is there. If a comment seems
pointless, only then should your response be an explanation in the code review
tool.

If a reviewer didn't understand some piece of your code, it's likely other
future readers of the code won't understand either. Writing a response in the
code review tool doesn't help future code readers, but clarifying your code or
adding code comments does help them.

## Think for Yourself {#think}

Writing a CL can take a lot of work. It's often really satisfying to finally
send one out for review, feel like it's done, and be pretty sure that no further
work is needed. So when a reviewer comes back with comments on things that could
be improved, it's easy to reflexively think the comments are wrong, the reviewer
is blocking you unnecessarily, or they should just let you submit the CL.
However, **no matter how certain you are** at this point, take a moment to step
back and consider if the reviewer is providing valuable feedback that will help
the codebase and Google. Your first question to yourself should always be, "Is
the reviewer correct?"

If you can't answer that question, it's likely the reviewer needs to clarify
their comments.

If you *have* considered it and you still think you're right, feel free to
respond with an explanation of why your method of doing things is better for the
codebase, users, and/or Google. Often, reviewers are actually providing
*suggestions* and they want you to think for yourself about what's best. You
might actually know something about the users, codebase, or CL that the reviewer
doesn't know. So fill them in; give them more context. Usually you can come to
some consensus between yourself and the reviewer based on technical facts.

## Resolving Conflicts {#conflicts}

Your first step in resolving conflicts should always be to try to come to
consensus with your reviewer. If you can't achieve consensus, see
[The Standard of Code Review](../reviewer/standard.md), which gives principles
to follow in such a situation.
