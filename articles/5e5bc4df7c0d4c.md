---
title: "ChatGPTの軽量モデル「GPT4ALL」をメモリ8GBのM1 MacBookAirで動かしてみた"
emoji: "⛳"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ChatGPT, GPT4ALL, chatbot]
published: true
---

## 1. 概要

以下の記事を見かけました。
GPT-3.5-TurboとLLaMAで学習したチャットボットがNomic AIから発表されたようです。
メモリ8GBのM1Macでも動作したらしく、ちょうど自分も持っていたので試してみました。(商用利用は不可なので注意)
https://gigazine.net/news/20230330-gpt4all/

## 2. 環境
- MacBook Air:M1,2020
- チップ:Apple M1
- メモリ:8GB
- macOS:Ventura 13.3

## 3. インストール

1. 以下にアクセス
https://github.com/nomic-ai/gpt4all

2. README.mdを見ると`Try it yourself`とあるのでそちらの`Direct Link`からbinファイルをダウンロード
![](/images/5e5bc4df7c0d4c/2023-04-05-05-12-31.png)

3. (1.)のリポジトリをクローン
`git clone git@github.com:nomic-ai/gpt4all.git`

4. クローンした中に`chat`ディレクトリがあるのでそこに先ほどダウンロードしたbinファイルを移動
![](/images/5e5bc4df7c0d4c/2023-04-05-05-18-33.png)

## 4. 実行

1. ターミナルを開き`gpt-4-all`に移動し`cd chat;./gpt4all-lora-quantized-OSX-m1`を実行
![](/images/5e5bc4df7c0d4c/2023-04-05-05-37-59.png)

2. 色々聞いてみる
日本語と英語で同じ質問をしてみましたが、やはり英語の方が良い出力が得られるようです。
![](/images/5e5bc4df7c0d4c/2023-04-05-05-51-33.png)

## 5. まとめ・感想

やってみると意外と簡単で、すぐに実行までできてしまいました。
精度としてはChatGPTにはもちろん敵いませんが、それでもこの能力を持つチャットボットがローカル環境で、自分のPC上で実行できてしまうということが素晴らしいと思います。しかもメモリ8GBで。
まさかこのような形でメモリをたくさん積んでおくんだったと思う日が来るとは思いもしませんでした。
他のチャットボットでもメモリさえあればより精度の良いものが動かせるという話もあるので試してみたいです。
