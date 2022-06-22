---
tableOfContents: true
---

# 出力

## はじめに

<!-- You can store your content as a JSON object or as a good old HTML string. Both work fine. And of course, you can pass both formats to the editor to restore your content. Here is an interactive example, that exports the content as HTML and JSON when the document is changed: -->

コンテンツは JSON オブジェクト、または古き良き HTML 文字列として保存できます。どちらも正常に動作します。もちろん、両方の形式をエディターに渡してコンテンツを復元することもできます。これは、ドキュメントが変更されたときにコンテンツを HTML および JSON としてエクスポートするインタラクティブな例です。

## 書き出し

### オプション1：JSON

<!-- JSON is probably easier to loop through, for example to look for a mention and it’s more like what Tiptap uses under the hood. Anyway, if you want to use JSON to store the content we provide a method to retrieve the content as JSON: -->

JSON はおそらく、たとえばメンションを探すためにループするのが簡単で、Tiptap が内部で使用するものに似ています。とにかく、JSON を使用してコンテンツを保存する場合は、コンテンツを JSON として取得するメソッドを提供します。

```js
const json = editor.getJSON()
```

<!-- You can store that in your database (or send it to an API) and restore the document initially like that: -->

これをデータベースに保存して（または API に送信して）、最初は次のようにドキュメントを復元できます。

```js
new Editor({
  content: {
    "type": "doc",
    "content": [
      // …
    ]
  },
})
```

<!-- Or if you need to wait for something, you can do it later through the editor instance: -->

または、何かを待つ必要がある場合は、後でエディターインスタンスを使用してそれを行うことができます。

```js
editor.commands.setContent({
  "type": "doc",
  "content": [
    // …
  ]
})
```

<!-- Here is an interactive example where you can see that in action: -->

これは、実際の動作を確認できるインタラクティブな例です。

<https://embed.tiptap.dev/preview/GuideContent/ExportJSON?hideSource>

### オプション2：HTML

<!-- HTML can be easily rendered in other places, for example in emails and it’s wildly used, so it’s probably easier to switch the editor at some point. Anyway, every editor instance provides a method to get HTML from the current document: -->

HTMLは他の場所、たとえば電子メールで簡単にレンダリングでき、頻繁に使用されるため、ある時点でエディターを切り替える方がおそらく、より簡単でしょう。すべてのエディターインスタンスは、現在のドキュメントからHTMLを取得するためのメソッドを提供します。

```js
const html = editor.getHTML()
```

<!-- This can then be used to restore the document initially: -->

これを使用して、最初にドキュメントを復元できます。

```js
new Editor({
  content: `<p>Example Text</p>`,
})
```

<!-- Or if you want to restore the content later (e. g. after an API call has finished), you can do that too: -->

後で（API呼び出しが終了した後などに）コンテンツを復元する場合は、次のようにすることもできます。

```js
editor.commands.setContent(`<p>Example Text</p>`)
```

<!-- Use this interactive example to fiddle around: -->

このインタラクティブな例を使用して、いじくりまわします。

<https://embed.tiptap.dev/preview/GuideContent/ExportHTML?hideSource>

### オプション3：Y.js

Our editor has top notch support for Y.js, which is amazing to add features like [realtime collaboration, offline editing, or syncing between devices](/guide/collaborative-editing).

Internally, Y.js stores a history of all changes. That can be in the browser, on a server, synced with other connected clients, or on a USB stick. But, it’s important to know that Y.js needs those stored changes. A simple JSON document is not enough to merge changes.

Sure, you can import existing JSON documents to get started and get a JSON out of Y.js, but that’s more like an import/export format. It won’t be your single source. That’s important to consider when adding Y.js for one of the mentioned use cases.

That said, it’s amazing and we’re about to provide an amazing backend, that makes all that a breeze.

私たちのエディターは `Y.js` をサポートしています。これは、リアルタイムコラボレーション、オフライン編集、デバイス間の同期などの機能を追加するのに最適です。

内部的には、`Y.js` はすべての変更の履歴を保存します。これは、ブラウザ、サーバー、接続されている他のクライアントとの同期、または USB スティックで行うことができます。ただし、Y.js にはこれらの保存された変更が必要であることを知っておくことが重要です。単純な JSON ドキュメントでは、変更をマージするのに十分ではありません。

もちろん、既存の JSON ドキュメントをインポートして開始し、`Y.js` から JSON を取得することはできますが、それはインポート/エクスポート形式に似ています。それはあなたの単一の情報源ではありません。上記のユースケースの1つに `Y.js` を追加する場合は、これを考慮することが重要です。

とは言うものの、それは驚くべきことであり、私たちは驚くべきバックエンドを提供しようとしています。

### Markdown はサポートしていません

<!-- Unfortunately, **tiptap doesn’t support Markdown as an input or output format**. We considered to add support for it, but those are the reasons why we decided to not do it: -->

残念ながら、tiptap は入力または出力形式として Markdown をサポートしていません。サポートを追加することを検討しましたが、それを行わないことにした理由は次のとおりです。

<!-- 
* Both, HTML and JSON, can have deeply nested structures, Markdown is flat.
* Markdown standards vary.
* Tiptap’s strength is customization, that doesn’t work very well with Markdown.
* There are enough packages to convert HTML to Markdown and vice-versa. -->

* HTML と JSON はどちらも、深くネストされた構造を持つことができ、Markdown はフラットです。
* 値下げ基準は異なります。
* Tiptap の強みはカスタマイズであり、Markdown ではうまく機能しません。
* HTML を Markdown に、またはその逆に変換するのに十分なパッケージがあります。

<!-- You should really consider to work with HTML or JSON to store your content, they are perfectly fine for most use cases. -->

HTML または JSON を使用してコンテンツを保存することを検討する必要があります。これらは、ほとんどのユースケースで完全に問題ありません。

<!-- If you still think you need Markdown, ProseMirror has an [example on how to deal with Markdown](https://prosemirror.net/examples/markdown/), [Nextcloud Text](https://github.com/nextcloud/text) uses Tiptap 1 to work with Markdown. Maybe you can learn from them. Or if you are looking for a really good Markdown editor, try [CodeMirror](https://codemirror.net/). -->

それでも Markdown が必要だと思われる場合は、ProseMirror に Markdown の処理方法の例があり、NextcloudText は Tiptap1 を使用して Markdown を操作します。多分あなたはそれらから学ぶことができます。または、本当に優れた Markdown エディターを探している場合は、CodeMirror を試してください。

That said, Tiptap does support [Markdown shortcuts](/examples/markdown-shortcuts) to format your content. Also you’re free to let your content look like Markdown, for example add a `#` before an `<h1>` with CSS.

そうは言っても、Tiptap はコンテンツをフォーマットするためのMarkdown ショートカットをサポートしています。また、コンテンツをMarkdown のように見せることもできます。たとえば、`<h1>` の前に `#` を記入します。

## 変化を聞く

<!-- If you want to continuously store the updated content while people write, you can [hook into events](/api/events). Here is an example how that could look like: -->

人々が書いている間、更新されたコンテンツを継続的に保存したい場合は、[イベントにフックする](/api/events) ことができます。これがどのように見えるかの例です。

```js
const editor = new Editor({
  // intial content
  content: `<p>Example Content</p>`,

  // triggered on every change
  onUpdate: ({ editor }) => {
    const json = editor.getJSON()
    // send the content to an API here
  },
})
```

## レンダリング

### オプション1：Tiptapの読み取り専用インスタンス

To render the saved content, set the editor to read-only. That’s how you can achieve the exact same rendering as it’s in the editor, without duplicating your CSS and other code.

オプション1：Tiptapの読み取り専用インスタンス
保存したコンテンツをレンダリングするには、エディターを読み取り専用に設定します。これにより、CSSやその他のコードを複製することなく、エディターとまったく同じレンダリングを実現できます。

<https://embed.tiptap.dev/preview/GuideContent/ReadOnly>

### Option 2: Generate HTML from ProseMirror JSON

If you need to render the content on the server side, for example to generate the HTML for a blog post which has been written in Tiptap, you’ll probably want to do just that without an actual editor instance.

That’s what the `generateHTML()` is for. It’s a helper function which renders HTML without an actual editor instance.

＃オプション2：ProseMirrorJSONからHTMLを生成する
サーバー側でコンテンツをレンダリングする必要がある場合、たとえば、Tiptapで記述されたブログ投稿のHTMLを生成する場合は、実際のエディターインスタンスなしでそれを実行することをお勧めします。

それgenerateHTML()が目的です。これは、実際のエディターインスタンスなしでHTMLをレンダリングするヘルパー関数です。

<https://embed.tiptap.dev/preview/GuideContent/GenerateHTML>

By the way, the other way is possible, too. The below examples shows how to generate JSON from HTML.

ちなみに、他の方法も可能です。以下の例は、HTMLからJSONを生成する方法を示しています。

<https://embed.tiptap.dev/preview/GuideContent/GenerateJSON>

## マイグレーション

If you’re migrating existing content to Tiptap we would recommend to get your existing output to HTML. That’s probably the best format to get your initial content into Tiptap, because ProseMirror ensures there is nothing wrong with it. Even if there are some tags or attributes that aren’t allowed (based on your configuration), Tiptap just throws them away quietly.

既存のコンテンツをTiptapに移行する場合は、既存の出力をHTMLに変換することをお勧めします。ProseMirrorは問題がないことを保証するため、これはおそらく最初のコンテンツをTiptapに取り込むための最良の形式です。（構成に基づいて）許可されていないタグや属性がある場合でも、Tiptapはそれらを静かに破棄します。

We’re about to go through a few cases to help with that, for example we provide a PHP package to convert HTML to a compatible JSON structure: [ueberdosis/prosemirror-to-html](https://github.com/ueberdosis/html-to-prosemirror).

これを支援するために、いくつかのケースを実行しようとしています。たとえば、HTMLを互換性のあるJSON構造に変換するPHPパッケージを提供します：ueberdosis/prosemirror-to-html。

[Share your experiences with us!](mailto:humans@tiptap.dev) We’d like to add more information here.

あなたの経験を私たちと共有してください！ここにさらに情報を追加したいと思います。

## セキュリティー

There is no reason to use one or the other because of security concerns. If someone wants to send malicious content to your server, it doesn’t matter if it’s JSON or HTML. It doesn’t even matter if you’re using Tiptap or not. You should always validate user input.

セキュリティ上の懸念から、どちらか一方を使用する理由はありません。誰かが悪意のあるコンテンツをサーバーに送信したい場合、それがJSONであるかHTMLであるかは関係ありません。Tiptapを使用しているかどうかは関係ありません。常にユーザー入力を検証する必要があります。
