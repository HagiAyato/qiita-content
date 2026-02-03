---
title: Vercelの登録をGitHubアカウントで行ったらブロックされた件+その対応メモ
tags:
  - トラブルシューティング
  - Vercel
private: false
updated_at: '2021-03-02T23:20:57+09:00'
id: 1f5d0aebcd4fe0b742e1
organization_url_name: null
slide: false
ignorePublish: false
---
# はじめに
私は[自分サイト](https://hagiayato.github.io/my-site/)、[自作静的webアプリ](https://hagiayato.github.io/my-site/works/)の公開をGitHub Pagesでおこなっておりますが、同じことを
Vercelでもおこなうことができます。
[こちらの動画](https://www.youtube.com/watch?v=UuX8xPx8rWo)でVercelに興味を持ち、GitHubのアカウントで登録(sigh up)をしたところ、
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/655112/59259278-daee-eb1f-5604-f6d1ef92fe3d.png)
<font color="red">「Error:This user account is blocked. Contact support@vercel.com for more information.」</font>
このようなエラーが出て、何やら登録がブロックされたようです。
ページを再読み込みしても登録がブロックされたため、色々と対応してみることにしました。
# 対応
### サポートにメールを出す
ブロックの詳細について、エラーメッセージではサポートにメールしてほしいと書いてありました。
ということで早速サポートにメールで

- ブロックの原因
- ブロックを解消するために私は何をすればいいか

を問い合わせてみました。(超久しぶりの英文メール)

```メール本文.txt
Dear customer support,

My name is (GitHubアカウント名), an IT engineer in Japan.
(※ここに、登録時に使ったアカウントのリンクを貼る)

I tried signing up to vercel by My GitHub account for learning.
However it's blocked with the following message.
「Error:This user account is blocked. Contact support@vercel.com for more information.」

So please let me know
・Why it's blocked?
・How can I succeed in signing up?

I look forward to hearing from you.

Sincerely,
```

### 返事が来た
メールを出したのは日本時間2021/3/2の夜でしたが、その1時間後にサポートから返信が来ました。
(サポートが迅速!)
その内容は…

```
// 申し訳ありませんが、あなたのアカウントは不正使用防止システムに引っ掛かりました。これはエラーのようです。
My apologies — your account was flagged by our abuse prevention system, it looks like this was an error.

// 不正使用防止システムによるブロックを、今解除しました。
I have now unblocked your account. 
```

このメールを受け取った後に再度Vercelにアクセスしたところ、問題なくVercelにログインできました。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/655112/8b5051a2-95f9-aea9-e320-606e4657e73b.png)

### 考えられる原因は…?
実はGitHubのアカウントでVercelに登録した際、"Continue with GitHub"ボタンを2度押したからかアカウント連携画面を2つ出しておりました。
これでアカウント連携処理が不正扱いになっていたのかもしれません。

# おわりに
Vercelの登録時にアカウントがロックされた場合は、エラーメッセージの通りにサポートにメールを送れば対応できます。同じような事象が発生した際の参考になればと思います。
