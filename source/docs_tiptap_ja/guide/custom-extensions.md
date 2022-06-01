---
tableOfContents: true
---

# カスタム拡張機能

## はじめに

One of the strengths of Tiptap is its extendability. You don’t depend on the provided extensions, it is intended to extend the editor to your liking.

With custom extensions you can add new content types and new functionalities, on top of what already exists or from scratch. Let’s start with a few common examples of how you can extend existing nodes, marks and extensions.

You’ll learn how you start from scratch at the end, but you’ll need the same knowledge for extending existing and creating new extensions.

Tiptapの強みの1つは、その拡張性です。提供されている拡張機能に依存するのではなく、エディターを好みに合わせて拡張することを目的としています。

カスタム拡張機能を使用すると、既存のものに加えて、または最初から、新しいコンテンツタイプと新しい機能を追加できます。既存のノード、マーク、および拡張機能を拡張する方法のいくつかの一般的な例から始めましょう。

最後に最初から始める方法を学びますが、既存の拡張機能を拡張したり、新しい拡張機能を作成したりするためにも同じ知識が必要です。

## 既存の拡張機能を拡張する

Every extension has an `extend()` method, which takes an object with everything you want to change or add to it.

Let’s say, you’d like to change the keyboard shortcut for the bullet list. You should start with looking at the source code of the extension, in that case [the `BulletList` node](https://github.com/ueberdosis/tiptap/blob/main/packages/extension-bullet-list/src/bullet-list.ts). For the bespoken example to overwrite the keyboard shortcut, your code could look like that:

すべての拡張機能には、extend()変更または追加したいすべてのものを含むオブジェクトを取得するメソッドがあります。

たとえば、箇条書きのキーボードショートカットを変更したいとします。拡張機能のソースコード（この場合はBulletListノード）を確認することから始める必要があります。キーボードショートカットを上書きする特注の例の場合、コードは次のようになります。

```js
// 1. Import the extension
import BulletList from '@tiptap/extension-bullet-list'

// 2. Overwrite the keyboard shortcuts
const CustomBulletList = BulletList.extend({
  addKeyboardShortcuts() {
    return {
      'Mod-l': () => this.editor.commands.toggleBulletList(),
    }
  },
})

// 3. Add the custom extension to your editor
new Editor({
  extensions: [
    CustomBulletList(),
    // …
  ],
})
```

The same applies to every aspect of an existing extension, except to the name. Let’s look at all the things that you can change through the extend method. We focus on one aspect in every example, but you can combine all those examples and change multiple aspects in one `extend()` call too.

名前を除いて、同じことが既存の拡張機能のすべての側面に当てはまります。extendメソッドで変更できるすべてのものを見てみましょう。extend()すべての例で1つの側面に焦点を当てていますが、これらすべての例を組み合わせて、1回の呼び出しで複数の側面を変更することもできます。

### 名前

The extension name is used in a whole lot of places and changing it isn’t too easy. If you want to change the name of an existing extension, you can copy the whole extension and change the name in all occurrences.

The extension name is also part of the JSON. If you [store your content as JSON](/guide/output#option-1-json), you need to change the name there too.

拡張名は多くの場所で使用されており、変更するのは簡単ではありません。既存の拡張機能の名前を変更する場合は、拡張機能全体をコピーして、すべての場所で名前を変更できます。

拡張子名もJSONの一部です。コンテンツをJSONとして保存する場合は、そこでも名前を変更する必要があります。

### 優先順位

The priority defines the order in which extensions are registered. The default priority is `100`, that’s what most extension have. Extensions with a higher priority will be loaded earlier.

優先度は、拡張機能が登録される順序を定義します。デフォルトの優先度は100、ほとんどの拡張機能が持っているものです。優先度の高い拡張機能は、より早くロードされます。

```js
import Link from '@tiptap/extension-link'

const CustomLink = Link.extend({
  priority: 1000,
})
```

The order in which extensions are loaded influences two things:

拡張機能がロードされる順序は、次の2つのことに影響します。

1. #### プラグインの順序

   ProseMirror plugins of extensions with a higher priority will run first.

   優先度の高い拡張機能のProseMirrorプラグインが最初に実行されます。

2. #### スキーマの順序

   The [`Link`](/api/marks/link) mark for example has a higher priority, which means it will be rendered as `<a href="…"><strong>Example</strong></a>` instead of `<strong><a href="…">Example</a></strong>`.

   Linkたとえば、マークの優先度は高くなります。つまり、の代わりにマークがレンダリングされ<a href="…"><strong>Example</strong></a>ます<strong><a href="…">Example</a></strong>。

### 設定

All settings can be configured through the extension anyway, but if you want to change the default settings, for example to provide a library on top of Tiptap for other developers, you can do it like that:

いずれにせよ、すべての設定は拡張機能を介して構成できますが、デフォルト設定を変更する場合、たとえば、他の開発者にTiptapの上にライブラリを提供する場合は、次のように行うことができます。

```js
import Heading from '@tiptap/extension-heading'

const CustomHeading = Heading.extend({
  addOptions() {
    return {
      ...this.parent?.(),
      levels: [1, 2, 3],
    }
  },
})
```

### ストレージ

At some point you probably want to save some data within your extension instance. This data is mutable. You can access it within the extension under `this.storage`.

ある時点で、拡張インスタンス内にデータを保存したいと思うかもしれません。このデータは変更可能です。内の拡張子からアクセスできますthis.storage。

```js
import { Extension } from '@tiptap/core'

const CustomExtension = Extension.create({
  name: 'customExtension',

  addStorage() {
    return {
      awesomeness: 100,
    }
  },

  onUpdate() {
    this.storage.awesomeness += 1
  },
})
```

Outside the extension you have access via `editor.storage`. Make sure that each extension has a unique name.

拡張機能の外部では、を介してアクセスできますeditor.storage。各拡張子に一意の名前が付いていることを確認してください。

```js
const editor = new Editor({
  extensions: [
    CustomExtension,
  ],
})

const awesomeness = editor.storage.customExtension.awesomeness
```

### スキーマ

tiptap works with a strict schema, which configures how the content can be structured, nested, how it behaves and many more things. You [can change all aspects of the schema](/api/schema) for existing extensions. Let’s walk through a few common use cases.

tiptapは、コンテンツの構造化、ネスト、動作などを構成する厳密なスキーマで機能します。既存の拡張機能のスキーマのすべての側面を変更できます。いくつかの一般的なユースケースを見ていきましょう。

The default `Blockquote` extension can wrap other nodes, like headings. If you want to allow nothing but paragraphs in your blockquotes, set the `content` attribute accordingly:

デフォルトのBlockquote拡張子は、見出しなどの他のノードをラップできます。ブロッククォートに段落のみを許可する場合は、contentそれに応じて属性を設定します。

```js
// Blockquotes must only include paragraphs
import Blockquote from '@tiptap/extension-blockquote'

const CustomBlockquote = Blockquote.extend({
  content: 'paragraph*',
})
```

The schema even allows to make your nodes draggable, that’s what the `draggable` option is for. It defaults to `false`, but you can override that.

スキーマでは、ノードをドラッグ可能にすることもできます。これがdraggableオプションの目的です。デフォルトはfalseですが、これをオーバーライドできます。

```js
// Draggable paragraphs
import Paragraph from '@tiptap/extension-paragraph'

const CustomParagraph = Paragraph.extend({
  draggable: true,
})
```

That’s just two tiny examples, but [the underlying ProseMirror schema](https://prosemirror.net/docs/ref/#model.SchemaSpec) is really powerful.

これはほんの2つの小さな例ですが、基盤となるProseMirrorスキーマは非常に強力です。

### 属性

You can use attributes to store additional information in the content. Let’s say you want to extend the default `Paragraph` node to have different colors:

属性を使用して、コンテンツに追加情報を格納できます。デフォルトParagraphノードを拡張してさまざまな色にする必要があるとします。

```js
const CustomParagraph = Paragraph.extend({
  addAttributes() {
    // Return an object with attribute configuration
    return {
      color: {
        default: 'pink',
      },
    },
  },
})

// Result:
// <p color="pink">Example Text</p>
```

That is already enough to tell Tiptap about the new attribute, and set `'pink'` as the default value. All attributes will be rendered as a HTML attribute by default, and parsed from the content when initiated.

Let’s stick with the color example and assume you want to add an inline style to actually color the text. With the `renderHTML` function you can return HTML attributes which will be rendered in the output.

This examples adds a style HTML attribute based on the value of `color`:

これで、Tiptapに新しい属性を通知'pink'し、デフォルト値として設定するのに十分です。すべての属性はデフォルトでHTML属性としてレンダリングされ、開始時にコンテンツから解析されます。

色の例に固執し、実際にテキストに色を付けるためにインラインスタイルを追加するとします。このrenderHTML関数を使用すると、出力にレンダリングされるHTML属性を返すことができます。

この例では、次の値に基づいてスタイルHTML属性を追加しますcolor。

```js
const CustomParagraph = Paragraph.extend({
  addAttributes() {
    return {
      color: {
        default: null,
        // Take the attribute values
        renderHTML: attributes => {
          // … and return an object with HTML attributes.
          return {
            style: `color: ${attributes.color}`,
          }
        },
      },
    }
  },
})

// Result:
// <p style="color: pink">Example Text</p>
```

You can also control how the attribute is parsed from the HTML. Maybe you want to store the color in an attribute called `data-color` (and not just `color`), here’s how you would do that:

HTMLから属性を解析する方法を制御することもできます。data-color（だけでなく）という属性に色を保存したいcolor場合は、次のようにします。

```js
const CustomParagraph = Paragraph.extend({
  addAttributes() {
    return {
      color: {
        default: null,
        // Customize the HTML parsing (for example, to load the initial content)
        parseHTML: element => element.getAttribute('data-color'),
        // … and customize the HTML rendering.
        renderHTML: attributes => {
          return {
            'data-color': attributes.color,
            style: `color: ${attributes.color}`,
          }
        },
      },
    }
  },
})

// Result:
// <p data-color="pink" style="color: pink">Example Text</p>
```

You can completly disable the rendering of attributes with `rendered: false`.

を使用して、属性のレンダリングを完全に無効にすることができますrendered: false。

#### 既存の属性を拡張する

If you want to add an attribute to an extension and keep existing attributes, you can access them through `this.parent()`.

In some cases, it is undefined, so make sure to check for that case, or use optional chaining `this.parent?.()`

拡張機能に属性を追加して既存の属性を保持する場合は、を介してそれらにアクセスできますthis.parent()。

未定義の場合もありますので、必ず確認するか、オプションのチェーンをご利用くださいthis.parent?.()

```js
const CustomTableCell = TableCell.extend({
  addAttributes() {
    return {
      ...this.parent?.(),
      myCustomAttribute: {
        // …
      },
    }
  },
})
```

### グローバル属性

Attributes can be applied to multiple extensions at once. That’s useful for text alignment, line height, color, font family, and other styling related attributes.

Take a closer look at [the full source code](https://github.com/ueberdosis/tiptap/tree/main/packages/extension-text-align) of the [`TextAlign`](/api/extensions/text-align) extension to see a more complex example. But here is how it works in a nutshell:

属性は、一度に複数の拡張機能に適用できます。これは、テキストの配置、行の高さ、色、フォントファミリ、およびその他のスタイル関連の属性に役立ちます。

拡張機能の完全なソースコードを詳しく見TextAlignて、より複雑な例を確認してください。しかし、これが一言で言えばどのように機能するかです：

```js
import { Extension } from '@tiptap/core'

const TextAlign = Extension.create({
  addGlobalAttributes() {
    return [
      {
        // Extend the following extensions
        types: [
          'heading',
          'paragraph',
        ],
        // … with those attributes
        attributes: {
          textAlign: {
            default: 'left',
            renderHTML: attributes => ({
              style: `text-align: ${attributes.textAlign}`,
            }),
            parseHTML: element => element.style.textAlign || 'left',
          },
        },
      },
    ]
  },
})
```

### HTMLのレンダリング

With the `renderHTML` function you can control how an extension is rendered to HTML. We pass an attributes object to it, with all local attributes, global attributes, and configured CSS classes. Here is an example from the `Bold` extension:

このrenderHTML関数を使用すると、拡張機能をHTMLにレンダリングする方法を制御できます。すべてのローカル属性、グローバル属性、および構成されたCSSクラスを含む属性オブジェクトをオブジェクトに渡します。Bold拡張機能の例を次に示します。

```js
renderHTML({ HTMLAttributes }) {
  return ['strong', HTMLAttributes, 0]
},
```

The first value in the array should be the name of HTML tag. If the second element is an object, it’s interpreted as a set of attributes. Any elements after that are rendered as children.

The number zero (representing a hole) is used to indicate where the content should be inserted. Let’s look at the rendering of the `CodeBlock` extension with two nested tags:

配列の最初の値は、HTMLタグの名前である必要があります。2番目の要素がオブジェクトの場合、属性のセットとして解釈されます。その後の要素はすべて子としてレンダリングされます。

数字のゼロ（穴を表す）は、コンテンツを挿入する場所を示すために使用されます。CodeBlock2つのネストされたタグを使用した拡張機能のレンダリングを見てみましょう。

```js
renderHTML({ HTMLAttributes }) {
  return ['pre', ['code', HTMLAttributes, 0]]
},
```

If you want to add some specific attributes there, import the `mergeAttributes` helper from `@tiptap/core`:

mergeAttributesそこに特定の属性を追加する場合は、以下からヘルパーをインポートします@tiptap/core。

```js
import { mergeAttributes } from '@tiptap/core'

// ...

renderHTML({ HTMLAttributes }) {
  return ['a', mergeAttributes(HTMLAttributes, { rel: this.options.rel }), 0]
},
```

### HTMLを解析する

The `parseHTML()` function tries to load the editor document from HTML. The function gets the HTML DOM element passed as a parameter, and is expected to return an object with attributes and their values. Here is a simplified example from the [`Bold`](/api/marks/bold) mark:

このparseHTML()関数は、HTMLからエディタドキュメントを読み込もうとします。この関数は、パラメーターとして渡されたHTML DOM要素を取得し、属性とその値を持つオブジェクトを返すことが期待されています。Boldマークからの簡略化された例を次に示します。

```js
parseHTML() {
  return [
    {
      tag: 'strong',
    },
  ]
},
```

This defines a rule to convert all `<strong>` tags to `Bold` marks. But you can get more advanced with this, here is the full example from the extension:

<strong>これは、すべてのタグをBoldマークに変換するルールを定義します。ただし、これを使用するとさらに高度になります。拡張機能の完全な例を次に示します。

```js
parseHTML() {
  return [
    // <strong>
    {
      tag: 'strong',
    },
    // <b>
    {
      tag: 'b',
      getAttrs: node => node.style.fontWeight !== 'normal' && null,
    },
    // <span style="font-weight: bold"> and <span style="font-weight: 700">
    {
      style: 'font-weight',
      getAttrs: value => /^(bold(er)?|[5-9]\d{2,})$/.test(value as string) && null,
    },
  ]
},
```

This checks for `<strong>` and `<b>` tags, and any HTML tag with an inline style setting the `font-weight` to bold.

As you can see, you can optionally pass a `getAttrs` callback, to add more complex checks, for example for specific HTML attributes. The callback gets passed the HTML DOM node, except when checking for the `style` attribute, then it’s the value.

You are wondering what’s that `&& null` doing? [ProseMirror expects `null` or `undefined` if the check is successful.](https://prosemirror.net/docs/ref/version/0.18.0.html#model.ParseRule.getAttrs)

[Pass `priority` to a rule](https://prosemirror.net/docs/ref/version/0.18.0.html#model.ParseRule.priority) to resolve conflicts with other extensions, for example if you build a custom extension which looks for paragraphs with a class attribute, but you already use the default paragraph extension.

これにより、タグと、を太字に設定するインラインスタイルのHTMLタグがチェックされ<strong>ます。<b>font-weight

ご覧のとおり、オプションでgetAttrsコールバックを渡して、特定のHTML属性などのより複雑なチェックを追加できます。コールバックは、属性をチェックする場合を除いて、HTML DOMノードに渡され、styleそれが値になります。

あなたはそれが何をしているのか疑問に思って&& nullいますか？ProseMirrorは、チェックが成功したnullかどうかを期待します。undefined

priorityルールに渡して、他の拡張機能との競合を解決します。たとえば、クラス属性を持つ段落を検索するカスタム拡張機能を作成したが、すでにデフォルトの段落拡張機能を使用している場合です。

#### getAttrs の使用

The `getAttrs` function you’ve probably noticed in the example has two purposes:

このgetAttrs例でおそらく気付いた関数には、次の2つの目的があります。

1. Check the HTML attributes to decide whether a rule matches (and a mark or node is created from that HTML). When the function returns `false`, it’s not matching.
2. Get the DOM Element and use the HTML attributes to set your mark or node attributes accordingly:

HTML属性をチェックして、ルールが一致するかどうかを判断します（そして、マークまたはノードがそのHTMLから作成されます）。関数がを返すときfalse、それは一致していません。
DOM要素を取得し、HTML属性を使用して、それに応じてマークまたはノード属性を設定します。

```js
parseHTML() {
  return [
    {
      tag: 'span',
      getAttrs: element => {
        // Check if the element has an attribute
        element.hasAttribute('style')
        // Get an inline style
        element.style.color
        // Get a specific attribute
        element.getAttribute('data-color')
      },
    },
  ]
},
```

You can return an object with the attribute as the key and the parsed value to set your mark or node attribute. We would recommend to use the `parseHTML` inside `addAttributes()`, though. That will keep your code cleaner.

マークまたはノード属性を設定するために、キーとしての属性と解析された値を持つオブジェクトを返すことができます。parseHTMLただし、内部を使用することをお勧めしaddAttributes()ます。これにより、コードがよりクリーンに保たれます。

```js
addAttributes() {
  return {
    color: {
      // Set the color attribute according to the value of the `data-color` attribute
      parseHTML: element => element.getAttribute('data-color'),
    }
  }
},
```

Read more about `getAttrs` and all other `ParseRule` properties in the [ProseMirror reference](https://prosemirror.net/docs/ref/#model.ParseRule).

ProseMirrorリファレンスgetAttrsのおよびその他すべてのParseRuleプロパティの詳細をお読みください。

### コマンド

```js
import Paragraph from '@tiptap/extension-paragraph'

const CustomParagraph = Paragraph.extend({
  addCommands() {
    return {
      paragraph: () => ({ commands }) => {
        return commands.setNode('paragraph')
      },
    }
  },
})
```

:::warning Use the commands parameter inside of addCommands
To access other commands inside `addCommands` use the `commands` parameter that’s passed to it.
:::

addCommands内のcommandsパラメーターを使用します
内部の他のコマンドにアクセスするには、渡されaddCommandsたパラメーターを使用します。commands

### キーボードショートカット

Most core extensions come with sensible keyboard shortcut defaults. Depending on what you want to build, you’ll likely want to change them though. With the `addKeyboardShortcuts()` method you can overwrite the predefined shortcut map:

ほとんどのコア拡張機能には、適切なキーボードショートカットのデフォルトが付属しています。構築したいものによっては、変更したい場合もあります。このaddKeyboardShortcuts()方法を使用すると、事前定義されたショートカットマップを上書きできます。

```js
// Change the bullet list keyboard shortcut
import BulletList from '@tiptap/extension-bullet-list'

const CustomBulletList = BulletList.extend({
  addKeyboardShortcuts() {
    return {
      'Mod-l': () => this.editor.commands.toggleBulletList(),
    }
  },
})
```

### 入力ルール

With input rules you can define regular expressions to listen for user inputs. They are used for markdown shortcuts, or for example to convert text like `(c)` to a `©` (and many more) with the [`Typography`](/api/extensions/typography) extension. Use the `markInputRule` helper function for marks, and the `nodeInputRule` for nodes.

入力ルールを使用すると、ユーザー入力をリッスンする正規表現を定義できます。これらは、マークダウンショートカットに使用されます。たとえば、拡張子が付い(c)た©（およびその他の）テキストに変換するために使用されます。マークとノードTypographyのヘルパー関数を使用します。markInputRulenodeInputRule

By default text between two tildes on both sides is transformed to ~~striked text~~. If you want to think one tilde on each side is enough, you can overwrite the input rule like this:

デフォルトでは、両側の2つのチルダ間のテキストは次のように変換されます。打たれたテキスト。両側に1つのチルダで十分だと考えたい場合は、次のように入力ルールを上書きできます。

```js
// Use the ~single tilde~ markdown shortcut
import Strike from '@tiptap/extension-strike'
import { markInputRule } from '@tiptap/core'

// Default:
// const inputRegex = /(?:^|\s)((?:~~)((?:[^~]+))(?:~~))$/

// New:
const inputRegex = /(?:^|\s)((?:~)((?:[^~]+))(?:~))$/

const CustomStrike = Strike.extend({
  addInputRules() {
    return [
      markInputRule({
        find: inputRegex,
        type: this.type,
      }),
    ]
  },
})
```

### ルールを貼り付けます

Paste rules work like input rules (see above) do. But instead of listening to what the user types, they are applied to pasted content.

There is one tiny difference in the regular expression. Input rules typically end with a `$` dollar sign (which means “asserts position at the end of a line”), paste rules typically look through all the content and don’t have said `$` dollar sign.

Taking the example from above and applying it to the paste rule would look like the following example.

貼り付けルールは、入力ルール（上記を参照）と同じように機能します。ただし、ユーザーが入力した内容を聞く代わりに、貼り付けられたコンテンツに適用されます。

正規表現には小さな違いが1つあります。入力ルールは通常、$ドル記号で終わります（これは、「行の終わりの位置をアサートする」ことを意味します）。貼り付けルールは通常、すべてのコンテンツを調べ、ドル記号はありません$。

上記の例を使用して貼り付けルールに適用すると、次の例のようになります。

```js
// Check pasted content for the ~single tilde~ markdown syntax
import Strike from '@tiptap/extension-strike'
import { markPasteRule } from '@tiptap/core'

// Default:
// const pasteRegex = /(?:^|\s)((?:~~)((?:[^~]+))(?:~~))/g

// New:
const pasteRegex = /(?:^|\s)((?:~)((?:[^~]+))(?:~))/g

const CustomStrike = Strike.extend({
  addPasteRules() {
    return [
      markPasteRule({
        find: pasteRegex,
        type: this.type,
      }),
    ]
  },
})
```

### イベント

You can even move your [event listeners](/api/events) to a separate extension. Here is an example with listeners for all events:

イベントリスナーを別の拡張機能に移動することもできます。すべてのイベントのリスナーの例を次に示します。

```js
import { Extension } from '@tiptap/core'

const CustomExtension = Extension.create({
  onCreate() {
    // The editor is ready.
  },
  onUpdate() {
    // The content has changed.
  },
  onSelectionUpdate({ editor }) {
    // The selection has changed.
  },
  onTransaction({ transaction }) {
    // The editor state has changed.
  },
  onFocus({ event }) {
    // The editor is focused.
  },
  onBlur({ event }) {
    // The editor isn’t focused anymore.
  },
  onDestroy() {
    // The editor is being destroyed.
  },
})
```

### これで何が利用できますか？

Those extensions aren’t classes, but you still have a few important things available in `this` everywhere in the extension.

thisこれらの拡張機能はクラスではありませんが、拡張機能のどこでも利用できる重要なものがいくつかあります。

```js
// Name of the extension, for example 'bulletList'
this.name

// Editor instance
this.editor

// ProseMirror type
this.type

// Object with all settings
this.options

// Everything that’s in the extended extension
this.parent
```

### ProseMirrorプラグイン（高度）

After all, Tiptap is built on ProseMirror and ProseMirror has a pretty powerful plugin API, too. To access that directly, use `addProseMirrorPlugins()

結局のところ、TiptapはProseMirror上に構築されており、ProseMirrorには非常に強力なプラグインAPIもあります。これに直接アクセスするには、を使用しますaddProseMirrorPlugins()。`.

#### 既存のプラグイン

You can wrap existing ProseMirror plugins in Tiptap extensions like shown in the example below.

以下の例に示すように、既存のProseMirrorプラグインをTiptap拡張機能でラップできます。

```js
import { history } from 'prosemirror-history'

const History = Extension.create({
  addProseMirrorPlugins() {
    return [
      history(),
      // …
    ]
  },
})
```

#### ProseMirrorAPIにアクセスします

To hook into events, for example a click, double click or when content is pasted, you can pass [event handlers](https://prosemirror.net/docs/ref/#view.EditorProps) to `editorProps` on the [editor](/api/editor#editor-props).

Or you can add them to a Tiptap extension like shown in the below example.

クリック、ダブルクリック、コンテンツの貼り付けなどのイベントにフックするには、エディターでイベントハンドラーを渡すことができます。editorProps

または、以下の例に示すように、それらをTiptap拡張機能に追加できます。

```js
import { Extension } from '@tiptap/core'
import { Plugin, PluginKey } from 'prosemirror-state'

export const EventHandler = Extension.create({
  name: 'eventHandler',

  addProseMirrorPlugins() {
    return [
      new Plugin({
        key: new PluginKey('eventHandler'),
        props: {
          handleClick(view, pos, event) { /* … */ },
          handleDoubleClick(view, pos, event) { /* … */ },
          handlePaste(view, event, slice) { /* … */ },
          // … and many, many more.
          // Here is the full list: https://prosemirror.net/docs/ref/#view.EditorProps
        },
      }),
    ]
  },
})
```

### ノードビュー（詳細）

For advanced use cases, where you need to execute JavaScript inside your nodes, for example to render a sophisticated interface around an image, you need to learn about node views.

They are really powerful, but also complex. In a nutshell, you need to return a parent DOM element, and a DOM element where the content should be rendered in. Look at the following, simplified example:

たとえば、画像の周囲に洗練されたインターフェイスをレンダリングするために、ノード内でJavaScriptを実行する必要がある高度なユースケースでは、ノードビューについて学習する必要があります。

それらは本当に強力ですが、複雑でもあります。簡単に言うと、親DOM要素と、コンテンツがレンダリングされるDOM要素を返す必要があります。次の簡略化された例を見てください。

```js
import Image from '@tiptap/extension-image'

const CustomImage = Image.extend({
  addNodeView() {
    return () => {
      const container = document.createElement('div')

      container.addEventListener('click', event => {
        alert('clicked on the container')
      })

      const content = document.createElement('div')
      container.append(content)

      return {
        dom: container,
        contentDOM: content,
      }
    }
  },
})
```

There is a whole lot to learn about node views, so head over to the [dedicated section in our guide about node views](/guide/node-views) for more information. If you are looking for a real-world example, look at the source code of the [`TaskItem`](/api/nodes/task-item) node. This is using a node view to render the checkboxes.

ノードビューについて学ぶことはたくさんあるので、詳細については、ノードビューに関するガイドの専用セクションに進んでください。実際の例を探している場合は、TaskItemノードのソースコードを確認してください。これは、ノードビューを使用してチェックボックスをレンダリングしています。

## 新しい拡張機能を作成する

You can build your own extensions from scratch and you know what? It’s the same syntax as for extending existing extension described above.

独自の拡張機能を最初から作成できますが、何を知っていますか？これは、上記の既存の拡張機能を拡張する場合と同じ構文です。

### ノードを作成する

If you think of the document as a tree, then [nodes](/api/nodes) are just a type of content in that tree. Good examples to learn from are [`Paragraph`](/api/nodes/paragraph), [`Heading`](/api/nodes/heading), or [`CodeBlock`](/api/nodes/code-block).

ドキュメントをツリーと考えると、ノードはそのツリーのコンテンツの一種にすぎません。学ぶべき良い例は、、、ParagraphまたはHeadingですCodeBlock。

```js
import { Node } from '@tiptap/core'

const CustomNode = Node.create({
  name: 'customNode',

  // Your code goes here.
})
```

Nodes don’t have to be blocks. They can also be rendered inline with the text, for example for [@mentions](/api/nodes/mention).

ノードはブロックである必要はありません。@mentionsのように、テキストとインラインでレンダリングすることもできます。

### マークを作成する

One or multiple marks can be applied to [nodes](/api/nodes), for example to add inline formatting. Good examples to learn from are [`Bold`](/api/marks/bold), [`Italic`](/api/marks/italic) and [`Highlight`](/api/marks/highlight).

たとえば、インラインフォーマットを追加するために、1つまたは複数のマークをノードに適用できます。学ぶべき良い例は、、Boldです。ItalicHighlight

```js
import { Mark } from '@tiptap/core'

const CustomMark = Mark.create({
  name: 'customMark',

  // Your code goes here.
})
```

### 拡張機能を作成する

Extensions add new capabilities to Tiptap and you’ll read the word extension here very often, even for nodes and marks. But there are literal extensions. Those can’t add to the schema (like marks and nodes do), but can add functionality or change the behaviour of the editor.

拡張機能はTiptapに新しい機能を追加し、ノードやマークの場合でも、ここで拡張機能という単語を頻繁に読みます。しかし、文字通りの拡張があります。これらは（マークやノードのように）スキーマに追加することはできませんが、機能を追加したり、エディターの動作を変更したりすることはできます。

A good example to learn from is probably [`TextAlign`](/api/extensions/text-align).

学ぶべき良い例はおそらくTextAlignです。

```js
import { Extension } from '@tiptap/core'

const CustomExtension = Extension.create({
  name: 'customExtension',

  // Your code goes here.
})
```

## 共有

When everything is working fine, don’t forget to [share it with the community](https://github.com/ueberdosis/tiptap/issues/819).

すべてが正常に機能している場合は、コミュニティと共有することを忘れないでください。
