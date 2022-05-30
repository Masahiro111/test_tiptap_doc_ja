---
tableOfContents: true
---

# アップグレードガイド

## はじめに

まず、Tiptap v1 はサポートされなくなり、それ以上のアップデートを受信しなくなります。

<!-- Yes, it’s tedious work to upgrade your favorite text editor to a new API, but we made sure you’ve got enough reasons to upgrade to the newest version. -->

お気に入りのテキストエディタを新しいAPIにアップグレードするのは面倒な作業ですが、最新バージョンにアップグレードする十分な理由があることを確認しました。

* Autocompletion in your IDE (thanks to TypeScript)
* Amazing documentation with 100+ pages and 100+ interactive examples
* Active development, new features in the making, new releases every week
* Tons of new extensions
* Well-tested code base

* IDEでのオートコンプリート（TypeScriptのおかげで）
* 100以上のページと100以上のインタラクティブな例を含むすばらしいドキュメント
* 活発な開発、作成中の新機能、毎週の新リリース
* たくさんの新しい拡張機能
* 十分にテストされたコードベース

<!-- The new API will look pretty familiar to you, but there are a ton of changes though. To make the upgrade a little bit easier, here is everything you need to know: -->

新しいAPIは見覚えがありますが、多くの変更があります。アップグレードを少し簡単にするために、知っておく必要のあるすべてがここにあります。

## Tiptap v1 をアンインストール

<!-- The whole package structure has changed, we even moved to another npm namespace, so you’ll need to remove the old version entirely before upgrading to Tiptap 2. -->

<!-- Otherwise you’ll run into an exception, for example “looks like multiple versions of prosemirror-model were loaded”. -->

パッケージ構造全体が変更され、別の npm 名前空間に移動したため、Tiptap v2 にアップグレードする前に古いバージョンを完全に削除する必要があります。

そうしないと、「 prosemirror-model の複数のバージョンがロードされたように見えます(looks like multiple versions of prosemirror-model were loaded)」などの例外が発生します。

```bash
npm uninstall tiptap tiptap-commands tiptap-extensions tiptap-utils
```

## Tiptap v2 をインストール

<!-- Once you have uninstalled the old version of Tiptap, install the new Vue 2 package and the starter kit: -->

古いバージョンの Tiptap をアンインストールしたら、新しい Vue2 パッケージとスターターキットをインストールします。

```bash
npm install @tiptap/vue-2 @tiptap/starter-kit
```

## Tiptap v2 を最新の状態に保つ

<!-- We are constantly releasing updates to Tiptap. -->

<!-- Unfortunately, for npm there is no integrated tool to easily update your dependencies, but you can use the `npm-check` package: -->

Tiptapのアップデートを常にリリースしています。

残念ながら、npmには依存関係を簡単に更新する統合ツールはありませんが、次の `npm-check` パッケージを使用できます。

```bash
npm install -g npm-check
npm-check -u
```

## ドキュメント、テキスト、段落の拡張子を明示的に登録する

tiptap 1 tried to hide a few required extensions from you with the default setting `useBuiltInExtensions: true`. That setting has been removed and you’re required to import all extensions. Be sure to explicitly import at least the [`Document`](/api/nodes/document), [`Paragraph`](/api/nodes/paragraph) and [`Text`](/api/nodes/text) extensions.

tiptap 1は、デフォルト設定でいくつかの必要な拡張機能を非表示にしようとしましたuseBuiltInExtensions: true。その設定は削除されており、すべての拡張機能をインポートする必要があります。少なくともDocument、ParagraphおよびText拡張子を明示的にインポートしてください。

```js
import Document from '@tiptap/extension-document'
import Paragraph from '@tiptap/extension-paragraph'
import Text from '@tiptap/extension-text'

new Editor({
  extensions: [
    Document,
    Paragraph,
    Text,
    // all your other extensions
  ],
})
```

And we removed some settings: `dropCursor`, `enableDropCursor`, and `enableGapCursor`. Those are separate extensions now: [`Dropcursor`](/api/extensions/dropcursor) and [`Gapcursor`](/api/extensions/gapcursor). You probably want to load them, but if you don’t, just ignore this.

そして、いくつかの設定を削除しました：dropCursor、、、。これらは現在、別個の拡張機能です：および。おそらくそれらをロードしたいのですが、ロードしない場合は、これを無視してください。enableDropCursorenableGapCursorDropcursorGapcursor

## ほとんどの拡張機能の新しい名前

We switched to lowerCamelCase, so there’s a lot type names that changed. If you stored your content as JSON you need to loop through it and rename them. Sorry for that one.

lowerCamelCaseに切り替えたため、多くのタイプ名が変更されました。コンテンツをJSONとして保存した場合は、コンテンツをループして名前を変更する必要があります。すみません。

| Old type              | New type               |
| --------------------- | ---------------------- |
| ~~`bullet_list`~~     | `bulletList`           |
| ~~`code_block`~~      | `codeBlock`            |
| ~~`hard_break`~~      | `hardBreak`            |
| ~~`horizontal_rule`~~ | `horizontalRule`       |
| ~~`list_item`~~       | `listItem`             |
| ~~`ordered_list`~~    | `orderedList`          |
| ~~`table_cell`~~      | `tableCell`            |
| ~~`table_header`~~    | `tableHeader`          |
| ~~`table_row`~~       | `tableRow`             |
| ~~`todo_list`~~       | `taskList` (new name!) |
| ~~`todo_item`~~       | `taskItem` (new name!) |

## 削除されたメソッド

We removed the `.state()` method. No worries though, it’s still available through `editor.state`.

.state()メソッドを削除しました。でも心配はいりません、それはまだを通して利用可能ですeditor.state。

## 新しい拡張API

In case you’ve built some custom extensions for your project, you’re required to rewrite them to fit the new API. No worries, you can keep a lot of your work though. The `schema`, `commands`, `keys`, `inputRules` and `pasteRules` all work like they did before. It’s just different how you register them.

プロジェクト用にいくつかのカスタム拡張機能を構築した場合は、新しいAPIに合うようにそれらを書き直す必要があります。心配はいりませんが、多くの作業を続けることができます。、、、schemaおよびすべてが以前commandsと同じように機能しkeysますinputRules。pasteRules登録方法が違うだけです。

```js
import { Node } from '@tiptap/core'

const CustomExtension = Node.create({
  name: 'custom_extension',
  addOptions() {
    …
  },
  addAttributes() {
    …
  },
  parseHTML() {
    …
  },
  renderHTML({ node, HTMLAttributes }) {
    …
  },
  addCommands() {
    …
  },
  addKeyboardShortcuts() {
    …
  },
  addInputRules() {
    …
  },
  // and more …
})
```

Read more about [all the nifty details building custom extensions](/guide/custom-extensions) in our guide.

ガイドでカスタム拡張機能を構築するためのすべての気の利いた詳細をお読みください。

## 名前を変更した設定とメソッド

[We renamed a lot of settings and methods](/api/editor). Hopefully you can migrate to the new API with search & replace. Here is a list of what changed:

多くの設定とメソッドの名前を変更しました。うまくいけば、検索と置換を使用して新しいAPIに移行できます。変更点のリストは次のとおりです。

| 旧名        | 新しい名前    |
| --------------- | ----------- |
| ~~`autoFocus`~~ | `autofocus` |

## 名前が変更されたコマンド

All new extensions come with specific commands to set, unset and toggle styles. So instead of `.bold()`, it’s now `.toggleBold()`. Also, we switched to lowerCamelCase, below are a few examples. Oh, and we renamed `todo_list`, to `taskList`, sorry for that one.

すべての新しい拡張機能には、スタイルを設定、設定解除、および切り替えるための特定のコマンドが付属しています。したがって.bold()、の代わりに、現在は.toggleBold()です。また、lowerCamelCaseに切り替えました。以下にいくつかの例を示します。ああ、名前を変更todo_listしましtaskListた。申し訳ありません。

| 以前のコマンド              | 新しいコマンド command                     |
| ------------------------ | ------------------------------- |
| `.redo()`                | `.redo()` (nothing changed)     |
| `.undo()`                | `.undo()` (nothing changed)     |
| ~~`.todo_list()`~~       | `.toggleTaskList()` (new name!) |
| ~~`.blockquote()`~~      | `.toggleBlockquote()`           |
| ~~`.bold()`~~            | `.toggleBold()`                 |
| ~~`.bullet_list()`~~     | `.toggleBulletList()`           |
| ~~`.code()`~~            | `.toggleCode()`                 |
| ~~`.code_block()`~~      | `.toggleCodeBlock()`            |
| ~~`.hard_break()`~~      | `.setHardBreak()`               |
| ~~`.heading()`~~         | `.toggleHeading()`              |
| ~~`.horizontal_rule()`~~ | `.setHorizontalRule()`          |
| ~~`.italic()`~~          | `.toggleItalic()`               |
| ~~`.link()`~~            | `.toggleLink()`                 |
| ~~`.ordered_list()`~~    | `.toggleOrderedList()`          |
| ~~`.paragraph()`~~       | `.setParagraph()`               |
| ~~`.strike()`~~          | `.toggleStrike()`               |
| ~~`.underline()`~~       | `.toggleUnderline()`            |
| …                        | …                               |

## MenuBar, BubbleMenu and FloatingMenu

MenuBar、BubbleMenuおよびFloatingMenu

Read the dedicated [guide on creating menus](/guide/menus) to migrate your menus.

メニューを移行するためのメニューの作成に関する専用ガイドをお読みください。

## コマンドをチェーンできるようになりました

Most commands can be combined to one call now. That’s shorter than separate function calls in most cases. Here is an example to make the selected text bold:

現在、ほとんどのコマンドを1つの呼び出しに組み合わせることができます。ほとんどの場合、これは個別の関数呼び出しよりも短くなります。選択したテキストを太字にする例を次に示します。

```js
editor.chain().toggleBold().focus().run()
```

The `.chain()` is required to start a new chain and the `.run()` is needed to actually execute all the commands in between. Read more about [the new Tiptap commands](/api/commands) in our API documentation.

は.chain()新しいチェーンを開始するため.run()に必要であり、はその間のすべてのコマンドを実際に実行するために必要です。新しいTiptapコマンドの詳細については、APIドキュメントをご覧ください。

## .focus（）がすべてのコマンドで呼び出されることはなくなりました

We tried to hide the `.focus()` command from you with Tiptap 1 and executed that on every command. That led to issues in specific use cases, where you want to run a command, but don’t want to focus the editor.

With Tiptap v2 you have to explicitly call the `focus()` and you probably want to do that in a lot of places. Here is an example:

.focus()Tiptap 1を使用してコマンドを非表示にしようとし、すべてのコマンドでそれを実行しました。これにより、特定のユースケースで問題が発生しました。コマンドを実行したいが、エディターに焦点を合わせたくない場合です。

Tiptap v2では、を明示的に呼び出す必要がfocus()あり、おそらく多くの場所でそれを実行したいと思うでしょう。次に例を示します。

```js
editor.chain().focus().toggleBold().run()
```

## Event callbacks have fewer parameters

イベントコールバックのパラメータは少なくなります

The new event callbacks have fewer parameters. The same things should be available through `this.` now. [Read more about events here.](/api/events)

新しいイベントコールバックには、より少ないパラメーターがあります。同じものが今まで利用できるはずですthis.。イベントの詳細については、こちらをご覧ください。

## コラボ編集

The reference implementation for collaborative editing uses Y.js now. That’s a whole different thing. You still can use the Tiptap 1 extension, but it’s up to you to adapt it to the new extension API. If you’ve done this, don’t forget to share it with us so we can link to it from here!

Read more about [the new collaborative editing experience](/guide/collaborative-editing) in our guide.

協調編集のリファレンス実装では、現在Y.jsを使用しています。それはまったく別のことです。Tiptap 1拡張機能は引き続き使用できますが、新しい拡張機能APIに適合させるかどうかはユーザー次第です。これを行った場合は、ここからリンクできるように、忘れずに共有してください。

ガイドで新しい協調編集体験の詳細をご覧ください。

## Marks don’t support node view anymore

マークはノードビューをサポートしなくなりました

For marks, node views are [not well supported in ProseMirror](https://discuss.prosemirror.net/t/there-is-a-bug-in-marks-nodeview/2722/2). There is also [a related issue](https://github.com/ueberdosis/tiptap/issues/613) for Tiptap 1. That’s why we removed it in Tiptap 2.

マークの場合、ノードビューはProseMirrorでは十分にサポートされていません。Tiptap 1にも関連する問題があります。そのため、Tiptap2で削除しました。

## スポンサーになる

tiptap wouldn’t exist without the funding of its community. If you fell in love with Tiptap, don’t forget to [become a sponsor](/sponsor) and make the maintenance, development and support sustainable.

In exchange, we’ll take you into our hearts, invite you to private repositories, add a `sponsor ♥` label to your issues and pull requests and more.

tiptapは、そのコミュニティの資金がなければ存在しませんでした。Tiptapに恋をした場合は、スポンサーになり、メンテナンス、開発、サポートを持続可能なものにすることを忘れないでください。

引き換えに、私たちはあなたを私たちの心に連れて行き、あなたをプライベートリポジトリに招待sponsor ♥し、あなたの問題にラベルを追加し、リクエストをプルします。
