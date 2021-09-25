---
title: "Github Pages & Hugoで無料爆速ブログ開設"
date: 2021-09-25T20:26:49+07:00
draft: false
---
## Repository作成

"{GithubアカウントID}.github.io"という名前でレポジトリを作成してClone。

IDに大文字が入っているとURLにも大文字が入って面倒なので、大文字の入ったIDは事前に変えておいた方が良い。

無料で利用したい場合はRepositoryを公開する設定で。

## Hugo 導入

公式のQuick Startガイドの通りに進める。
https://gohugo.io/getting-started/quick-start/

```hugo new site quickstart``` で作成したディレクトリの中身を全て、Repository作成でCloneしたフォルダへ移す。

途中の手順で生成した.mdファイルに記事内容をマークダウン形式で書く。

draft: true となっている部分を false に変えると記事が公開される。

```
---
title: "記事投稿テスト"
date: 2021-09-25T19:12:55+07:00
draft: false
---

## 大見出し - titleがH1タグ要素になるのでH2タグから使う

### 小見出し

この文章はダミーです。文字の大きさ、量、字間、行間等を確認するために入れています。この文章はダミーです。文字の大きさ、量、字間、行間等を確認するために入れています。この文章はダミーです。
```

設定の languageCode は"ja-jp"に変更。

テーマは公式ドキュメントの手順で使用されているanankeではなくm10cを使用。

## 自動デプロイ準備

https://gohugo.io/hosting-and-deployment/hosting-on-github/

".github/workflows/gh-pages.yml"というファイルを作成し、
上記のページに記載されているGithub Actionsのコードをコピペ
```
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

## デプロイ実行

レポジトリの中身を全てmainブランチにpushする。
```
git add .
git commit -m "first commit"
git push origin main
```

成功すると、gh-pagesというブランチが生成され、そちらに静的なページがビルドされている。

## 公開設定

Githubのレポジトリ画面からSettings -> Pages を開き、Sourceとして gh-pages ブランチ、参照先は root を選択。
![GithubのSettingsのスクリーンショット](/images/github-screenshot_20210925.png)

設定後、少し間をおいて反映される。


爆速！

