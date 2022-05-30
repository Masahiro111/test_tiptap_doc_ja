---
tableOfContents: true
---

# 貢献

## はじめに

tiptap would be nothing without its lively community. Contributions have always been and will always be welcome. Here is a little bit you should know, before you send your contribution:

活気のあるコミュニティがなければ、tiptapは何もありません。貢献はこれまでも、そしてこれからも歓迎されます。投稿を送信する前に、知っておくべきことが少しあります。

## 歓迎する例

* Failing regression tests as bug reports
* Documentation improvements, e. g. fix a typo, add a section
* New features for existing extensions, e. g. a new configureable option
* Well explained, non-breaking changes to the core

バグレポートとしての回帰テストの失敗
タイプミスの修正、セクションの追加など、ドキュメントの改善
既存の拡張機能の新機能、たとえば新しい構成可能なオプション
よく説明された、コアへの非破壊的な変更

## マージされません

* New extensions, which we then need to support and maintain

新しい拡張機能。サポートと保守が必要です。

## アイデアを提出する

Make sure to open an issue and outline your idea first. We’ll get back to you quickly and let you know if there is a chance we can merge your contribution.

必ず問題を開いて、最初にアイデアの概要を説明してください。すぐにご連絡いたします。寄付を統合できる可能性がある場合はお知らせします。

## 開発環境をセットアップする

It’s not too hard to tinker around with the official repository. You’ll need [Git](https://github.com/git-guides/install-git), [Node and NPM](https://nodejs.org/en/download/) installed. Here is what you need to do then:

1. Copy the code to your local machine: `$ git clone git@github.com:ueberdosis/tiptap.git`
2. Install dependencies: `$ npm install`
3. Start the development environment: `$ npm run start`
4. Open <http://localhost:3000> in your favorite browser.
5. Start playing around!

公式リポジトリをいじくり回すのはそれほど難しくありません。Git、Node、NPMがインストールされている必要があります。次に行う必要があることは次のとおりです。

1. コードをローカルマシンにコピーします。$ git clone
 git@github.com:ueberdosis/tiptap.git
2. 依存関係をインストールします。$ npm install
3. 開発環境を開始します。$ npm run start
4. お気に入りのブラウザでhttp：// localhost：3000を開きます。
5.遊んでみよう！

## 私たちのコードスタイル

There is an eslint config that ensures a consistent code style. To check for errors, run `$ npm run lint`. That’ll be checked when you send a pull request, too. Make sure it’s passing, before sending a pull request.

一貫したコードスタイルを保証するeslint構成があります。エラーをチェックするには、を実行し$ npm run lintます。プルリクエストを送信するときにもチェックされます。プルリクエストを送信する前に、合格していることを確認してください。

## エラーのテスト

Your pull request will automatically execute all our existing tests. Make sure that they all pass, before sending a pull request. Run all tests locally with `$ npm run test` or run single tests (e. g. when writing new ones) with `$ npm run test:open`.

プルリクエストは、既存のすべてのテストを自動的に実行します。プルリクエストを送信する前に、すべて合格していることを確認してください。を使用してすべてのテストをローカルで$ npm run test実行するか、を使用して単一のテストを実行します（新しいテストを作成する場合など）$ npm run test:open。

## その他の質問

さらに質問がありますか？リポジトリに新しい問題またはディスカッションを作成します。折り返しご連絡いたします。
