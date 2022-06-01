---
tableOfContents: true
---

# スタイリング

## はじめに

tiptap is headless, that means there is no styling provided. That also means, you are in full control of how your editor looks. The following methods allow you to apply custom styles to the editor.

tiptapはヘッドレスです。つまり、スタイリングは提供されません。これは、エディターの外観を完全に制御できることも意味します。次のメソッドを使用すると、カスタムスタイルをエディターに適用できます。

## Option 1: Style the plain HTML

オプション1：プレーンHTMLのスタイルを設定する

The whole editor is rendered inside of a container with the class `.ProseMirror`. You can use that to scope your styling to the editor content:

エディター全体は、クラスが含まれるコンテナー内にレンダリングされます.ProseMirror。これを使用して、スタイリングをエディターのコンテンツにスコープすることができます。

```css
/* Scoped to the editor */
.ProseMirror p {
  margin: 1em 0;
}
```

If you’re rendering the stored content somewhere, there won’t be a `.ProseMirror` container, so you can just globally add styling to the used HTML tags:

保存されたコンテンツをどこかにレンダリングする場合、.ProseMirrorコンテナは存在しないため、使用するHTMLタグにスタイルをグローバルに追加できます。

```css
/* Global styling */
p {
  margin: 1em 0;
}
```

## Option 2: Add custom classes

オプション2：カスタムクラスを追加する

You can control the whole rendering, including adding classes to everything.

すべてにクラスを追加することを含め、レンダリング全体を制御できます。

### Extensions

拡張機能

Most extensions allow you to add attributes to the rendered HTML through the `HTMLAttributes` option. You can use that to add a custom class (or any other attribute). That’s also very helpful, when you work with [Tailwind CSS](https://tailwindcss.com/).

ほとんどの拡張機能では、オプションを使用して、レンダリングされたHTMLに属性を追加できHTMLAttributesます。これを使用して、カスタムクラス（またはその他の属性）を追加できます。これは、 TailwindCSSを使用する場合にも非常に役立ちます。

```js
new Editor({
  extensions: [
    Document,
    Paragraph.configure({
      HTMLAttributes: {
        class: 'my-custom-paragraph',
      },
    }),
    Heading.configure({
      HTMLAttributes: {
        class: 'my-custom-heading',
      },
    }),
    Text,
  ],
})
```

The rendered HTML will look like that:

レンダリングされたHTMLは次のようになります。

```html
<h1 class="my-custom-heading">Example Text</p>
<p class="my-custom-paragraph">Wow, that’s really custom.</p>
```

If there are already classes defined by the extensions, your classes will be added.

拡張機能によって定義されたクラスがすでに存在する場合は、クラスが追加されます。

### エディター

You can even pass classes to the element which contains the editor like that:

次のようなエディターを含む要素にクラスを渡すこともできます。

```js
new Editor({
  editorProps: {
    attributes: {
      class: 'prose prose-sm sm:prose lg:prose-lg xl:prose-2xl mx-auto focus:outline-none',
    },
  },
})
```

### TailwindCSSを使用

The editor works fine with Tailwind CSS, too. Find an example that’s styled with the `@tailwindcss/typography` plugin below.

エディターはTailwindCSSでも正常に動作します。@tailwindcss/typography以下のプラグインでスタイル設定された例を見つけてください。

<https://embed.tiptap.dev/preview/Experiments/Tailwind>

## Option 3: Customize the HTML

オプション3：HTMLをカスタマイズする

Or you can customize the markup for extensions. The following example will make a custom bold extension that doesn’t render a `<strong>` tag, but a `<b>` tag:

または、拡張機能のマークアップをカスタマイズできます。<strong>次の例では、タグではなくタグをレンダリングするカスタムの太字の拡張機能を作成します<b>。

```js
import Bold from '@tiptap/extension-bold'

const CustomBold = Bold.extend({
  renderHTML({ HTMLAttributes }) {
    // Original:
    // return ['strong', HTMLAttributes, 0]
    return ['b', HTMLAttributes, 0]
  },
})

new Editor({
  extensions: [
    // …
    CustomBold,
  ],
})
```

You should put your custom extensions in separate files, but I think you got the idea.

カスタム拡張機能は別々のファイルに入れる必要がありますが、私はあなたがその考えを理解したと思います。
