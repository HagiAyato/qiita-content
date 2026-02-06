---
title: GitHub Pagesの更新環境構築でハマった話（GA対応とGitHub Actions移行）
tags:
  - GitHubPages
  - Jekyll
  - GitHubActions
  - Ruby
  - GoogleAnalytics
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

## やりたかったこと
GitHub Pagesで運用しているこのサイトに、Google Analytics (GA) のプライバシーポリシー対応（Cookie同意バナーの設置など）を行おうとしました。

## 久しぶりのローカル更新作業
私のサイトは久しく更新をしていませんでした。
その間にメインPCを新しいPCに変更していたので、また一からローカルでの更新作業環境を構築することにしました。
最新の Ruby (バージョン 3.4) をインストールし、 `bundle install` を叩いたのですが、ここから長い戦いが始まりました。

## 環境構築でのバージョン競合とエラー
Ruby 3.4 が新しすぎたのか、既存の `Gemfile` の設定ではエラーが多発しました。

1. **Bundlerのバージョン不整合**: 古い Bundler の指定が残っておりエラー。
2. **wdm のビルドエラー**: Windows でのディレクトリ監視用ライブラリ `wdm` が古く、ビルドに失敗。必須ではないため削除して対応。
3. **標準ライブラリの変更**: Ruby 3.4 から `csv`, `webrick`, `base64` などが標準ライブラリから外れており、Jekyll の起動時に `LoadError` が発生。これらを `Gemfile` に明示的に追加する必要がありました。

```ruby
gem "csv"
gem "webrick"
gem "base64"
```

## GitHub Pages から GitHub Actions への移行
ローカル環境はやっと動くようになりましたが、ここで新たな問題が。
ローカルは **Jekyll 4.1** (Ruby 3.4) ですが、GitHub Pages の標準ビルド環境は **Jekyll 3.9** 系です。
このまま Push してもバージョンの不一致でビルドが失敗してしまいます。

そこで、[GitHub Actions](https://docs.github.com/ja/actions) を使って、ローカルと同じ環境でビルドしてデプロイする方式に移行しました。

1. `.github/workflows/jekyll.yml` を作成し、Ruby 3.4 でビルドするワークフローを記述。
2. `Gemfile.lock` に Linux 用のプラットフォーム情報を追加 (`bundle lock --add-platform x86_64-linux`)。
3. GitHub のリポジトリ設定で、デプロイ元を「GitHub Actions」に変更。

これでようやく、最新環境でサイトを更新できるようになりました。

## あとがき
やり始めた作業が想定よりも多くかかり、かつ途中で詰まった今回の作業。  
しかしながらあるツールを使ったことで作業時間1-2時間程度で完了しました。  
そのツールが、**[Gemini Code Assist](https://marketplace.visualstudio.com/items?itemName=Google.geminicodeassist)**です。  
  
生成AIにコードを書かせたり、トラブルシューティングをさせたりすることができるのです。  
(この記事のベースを作ったのもGemini Code Assist)  
もしも数年前、生成AIがなかったころに同じことをしていたら、おそらく倍以上の時間を掛けていたことでしょう。  
思えば、最近の仕事でもコードを書くために生成AIを用いています。  
もはや、生成AIなしの開発には戻れません…