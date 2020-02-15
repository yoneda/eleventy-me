---
title: Github のリポジトリを一括で削除する
description: Github API を使ってGithub の整理をする
date: 2020-02-04
tags:
  - test
layout: layouts/post.njk
---

みなさんこんにちは。ただいま休職中で結構時間があります。自分のGithub のアカウントは2014年6月に作成されて以降もう6年が経とうとしています。整理整頓をしておらず、リポジトリ数が341個にまで増えてしまっていたので整理をしてみました。

### Github REST API を使ってみる
普段ウェブブラウザを通してGithub にアクセスしていますが、APIでの操作も可能です。Github のAPIのドキュメントはここにまとまっています。大変見やすいドキュメントに仕上がっているので、自分でREST API を設計するときには是非参考にしたいですね。例えば、特定のユーザ情報を取得したいときにはcurlで以下のように取得できます。

``` text/
curl https://api.github.com/users/yoneda
```

ドキュメントを見ればわかるように、特定のユーザのリポジトリを取得したいときは`https://api.github.com/users/:username/repos` にGETリクエストを、特定のユーザのフォロワーを取得したいときは`https://api.github.com/users/:username/followers` にGETリクエストを送ればOKです。


### Python で複数のリポジトリを一括で削除する
Github REST API には、特定のリポジトリを削除する機能も用意されています。ドキュメント中の`DELETE /repos/:owner/:repo`これは、`https://api.github.com/users/:username/repos`にDELETEリクエストを送れば、そのリポジトリが削除できることを意味します。例えば、ユーザー名がtanakaで、リポジトリ名がjanken-appだった場合は、`https://api.github.com/users/tanaka/janken-app` となります。削除が成功すれば、ResponseのStatusは204が返ってきます。この操作には管理者権限が必要で、OAuth認証が必須であることに注意が必要です。Python で複数のリポジトリを削除するスクリプトを書いてみました。

```python/
# coding:utf-8
import requests

repos = ["repo1", "repo2", "repo3"]
url = "https://api.github.com/repos/yoneda/"
headers = {"Authorization":"token [enter your token]"}
for repo in repos:
  r = requests.delete(url+repo, headers=headers)
  if r.status_code==204:
    print(f"delete succeeded! {repo}")
```

### 整理した感想
リポジトリを整理した結果、341個あったリポジトリが22個になりました。
削除したリポジトリには以下のような特徴がありました。
* 特定の技術を勉強するためだけにつくったもの
* リポジトリを作成したものの何もしてないもの
* 技術書を写経するときに使ったもの

プログラミングを勉強しはじめた頃は右も左もわからないので、
とりあえずリポジトリを作成してcontributionsが緑に変わって満足している自分がいました。
最近では考え方が変わってきて、

* よく利用されているライブラリに修正のリクエストを送る
* よく利用されているライブラリの議論に参加する
* 自らユーザの課題を解決するプロジェクトをはじめる

Github に正しい使い方などはないです。自分の勉強のために使うより、身近な技術的/社会的な課題を解決するために使ったほうが有意義じゃないでしょうか。今後は、自分の好きなライブラリの開発に参加したり、著名なドキュメントの翻訳をしたりしていきたいですね。

それでは、みなさん良いGithubライフを✋
