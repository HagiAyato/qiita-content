---
title: '[VBA,VB6]GoToとラベルの使いどころ'
tags:
  - VBA
  - VBAマクロ
  - goto文
  - VB6
  - VB6.0
private: false
updated_at: '2021-01-14T22:41:25+09:00'
id: 8adce79fb79dade4c729
organization_url_name: null
slide: false
ignorePublish: false
---
[本記事は、自分サイトのこの記事と同一内容です。](https://hagiayato.github.io/my-site/jekyll/update/2021/01/14/GoToAndLabel.html)
# はじめに
皆さんはプログラムでGoToとラベルを使っていますか?
~~GoToトラベルではありません~~
…GoToは下記理由であまり使われません。

 - 読みにくい・処理を理解しにくいコードになる
 - if,for,while,try-catchで代替できる

でも言語によっては、GoToを使わないといけない場面が出てきます。
その一つが、VBAおよびVB6のようなVB.net以前のVBです。
# Case1：例外
VBAおよびVB6,VB5には、try-catch構文がありません。
代わりに、```On Error文```があるのでこれをGoToと併せて使いましょう。

```vb
On Error GoTo Catch ' Try{ に相当

' 例外検知をしたい処理
GoTo Finally

Catch: ' }catch(Exception e){ に相当

' 例外処理

Finally: ' }Finally{ に相当
On Error Resume Next ' Finallyの中では例外を無視(そもそも出さないようにコードを書くこと)
' メモリ開放など例外発生に関係なく最後にやる処理
```
ちなみに最初から```On Error Resume Next```により例外を無視することができます。
この場合、例外および例外による意図しない動作の分析がしにくくなるので要注意。

# Case2：ループのContinue
forやwhileループについて、途中で処理とループを打ち切るbreak相当の処理はあります。
```Exit For/Do```を使うだけ。
問題は途中で処理を打ち切り次のループに移るcontinueがないということです。
ここに関してはGoToとラベルを使いましょう。

```vb
Dim i As Integer

For i = 初期値 To 終了値 Step 増分
    ' ループ内処理1
    ' Continue判定 条件を満たす場合はループ内処理2をしない
    If Continue条件 Then GoTo Continue
    ' Break判定 条件を満たす場合はここで処理打ち切り
    If Break条件 Then Exit For
    ' ループ内処理2
Continue:
Next
```

# おわりに
GoToとラベルはVBA、VB6ではまだまだ使う機会が多いですし、
これらの開発をしなくても、VB6を.netに置き換えるためにコードを読むことがあり得ます。
最小限、かつ分かりやすく使っていきましょう。
