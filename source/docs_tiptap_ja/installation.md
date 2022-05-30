---
tableOfContents: true
---

# インストール

## はじめに

Tiptap is framework-agnostic and even works with Vanilla JavaScript (if that’s your thing). The following integration guides help you integrating Tiptap in your JavaScript project.

Tiptap はフレームワークに依存せず、Vanilla JavaScriptでも機能します（それがあなたのものである場合）。次の統合ガイドで、JavaScript プロジェクトに Tiptap を統合するのに役立ててください。

＃統合ガイド

## 統合ガイド
<!-- * [CDN](/installation/cdn) -->
* [React](/installation/react)
* [Next.js](/installation/nextjs)
* [Vue 3](/installation/vue3)
* [Vue 2](/installation/vue2)
* [Nuxt.js](/installation/nuxt)
* [Svelte](/installation/svelte)
* [Alpine.js](/installation/alpine)
* [PHP](/installation/php)

### コミュニティの取り組み

* [Angular](https://github.com/sibiraj-s/ngx-tiptap)
* [SolidJS](https://github.com/LXSMNSYC/solid-tiptap)

## バニラJavaScript

<!-- You are using plain JavaScript or a framework that is not listed here? No worries, we provide everything you need. -->

プレーン JavaScript またはここにリストされていないフレームワークを使用していますか？心配いりません、私たちはあなたが必要とするすべてを提供します。

### 1.依存関係をインストール

For the following example you will need `@tiptap/core` (the actual editor) and `@tiptap/starter-kit`.

The StarterKit doesn’t include all, but the most common extensions.

@tiptap/core次の例では、（実際のエディター）とが必要になり@tiptap/starter-kitます。

StarterKitにはすべてが含まれているわけではありませんが、最も一般的な拡張機能が含まれています。

```bash
npm install @tiptap/core @tiptap/starter-kit
```

### 2. 2.マークアップを追加

Add the following HTML where you want the editor to be mounted:

エディターをマウントする場所に次のHTMLを追加します。

```html
<div class="element"></div>
```

### 3.エディターを初期化

Everything is in place now, so let’s set up the actual editor now. Add the following code to your JavaScript:

これですべてが整ったので、実際のエディターをセットアップしましょう。次のコードをJavaScriptに追加します。

```js
import { Editor } from '@tiptap/core'
import StarterKit from '@tiptap/starter-kit'

new Editor({
  element: document.querySelector('.element'),
  extensions: [
    StarterKit,
  ],
  content: '<p>Hello World!</p>',
})
```

Open your project in the browser to see Tiptap in action. Good work!

ブラウザでプロジェクトを開き、Tiptapの動作を確認します。よくできました！
