---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides, markdown enabled
title: プログラマーもすなるVue.jsといふものを以てブログというものを作らんとて作るなり
info: |
  [UV Study : Vue\.js LT会 \- connpass](https://uniquevision.connpass.com/event/311383/)のスライドです。
# apply any unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: 
lineNumbers: true
---

# プログラマーもすなる<br>Vue.jsといふものを以て<br>ブログというものを作らんとて<br>作るなり

2024-04-23@UV Study : Vue.js LT会

Yuhei FUJITA

---

# ふじた。ひらがなみっつでふじた。<br>よぶときはふじたさん

<div class="grid grid-cols-4 gap-4">

<div>

![icon](https://github.com/YuheiFUJITA.png)

</div>
<div class="col-span-3">

- 名前: Yuhei FUJITA（<ruby>藤<rt>ふじ</rt></ruby><ruby>田<rt>た</rt></ruby> <ruby>悠<rt>ゆう</rt></ruby><ruby>平<rt>へい</rt></ruby>）
- X(Twitter): [@Yuhei_FUJITA](https://twitter.com/Yuhei_FUJITA)
- GitHub: [@YuheiFUJITA](https://github.com/YuheiFUJITA)
- コミュニティ運営: [Vue Fes Japan](https://vuefes.jp/) / [VS Code Meetup](https://vscode.connpass.com/) / [ChatGPT Community(JP)](https://chatgpt.connpass.com/) / [フロントエンドカンファレンス北海道2024](https://fortee.jp/frontend-conf-hokkaido-2024)
- 趣味: [自然淘汰キャンプ](https://x.com/Yuhei_FUJITA/status/1748854045664829587) / [フィルムカメラ](https://www.instagram.com/yuhei_fujita.film/) / [SIGMA fp L買うよう背中を<ruby>押された<rt>どつかれた</rt></ruby>](https://twitter.com/Yuhei_FUJITA/status/1780934056479494332)

</div>
</div>

---
layout: statement
---

# ちょっと余談

---

# 私はVue 2（近代文）のことを古文と言いました
<Tweet id="1777161433295536564" scale="1.5" />

---
layout: iframe-left
url: https://011.vuejs.org/
---

# 書き方に歴史を感じる

## `var` や…
```js
var vm = new Vue({
  /* options */ 
})
```

## `v-repeat` や…

> Directives can encapsulate arbitrary DOM manipulations. For example `v-attr` manipulates an element’s attributes, <span color="red">`v-repeat`</span> clones an element based on an Array, `v-on` attaches event listeners… we will cover them later.


---
layout: statement
---

# ブログを作りたい

---

# 最初に検討した構成

- Nuxt 3
  - ここは安定のNuxt 3
  - Nuxt 3のフルスタック開発楽しい
- Vuetify 3
  - UIフレームワーク
  - いろいろあったけどもうコンポーネント揃ってきた
- microCMS<span v-click>←ここで困った</span>
  - ヘッドレスCMS

---

# やりたいこと

- カテゴリー
  - `日常（10）` みたいな感じで記事数を表示したい
  - できないことはない<span v-click>（API叩きまくることになる）</span>
- タグ
  - `Vue.js（10）` みたいな感じで記事数を表示したい
  - できないことはない<span v-after>（API叩きまくることになる）</span>
- バージョン管理
  - 有料プランにすればできる<span v-click>（4,900円〜/月）</span>

---

# とか言ってたら公式からリプが飛んできた

<dev class="grid grid-cols-2 gap-4">

<div>

<Tweet id="1744215194442842553" />

これでやろうと思ったらカテゴリーやタグごとにAPI叩きまくることになる（技術的には可能）

</div>
<div>

<Tweet id="1744587005181460823" scale="1" />

</div>
</dev>

---

# とはいえ個人で使うにはさすがに高すぎる

安くても4,900円/月、ひぃ

![](/images/microcms-pricing.png)

---
layout: iframe-left
url: https://content.nuxt.com/
---

# Nuxt Content

- ファイルベースのCMS
  - 記事をgit管理できる
- 様々なファイルフォーマットをサポート
  - Markdown<span v-click>←記事</span>
  - YAML<span v-click>←カテゴリー・タグ</span>
  - CSV
  - JSON

---

# セットアップ

## Nuxt Contentを追加

```bash
npx nuxi@latest module add content
```

## `nuxt.config.ts` に設定を追加

````md magic-move
```ts
export default defineNuxtConfig({
  modules: [
  ],
})
```
```ts
export default defineNuxtConfig({
  modules: [
    '@nuxt/content' // `modules` にNuxt Contentを追加
  ],
})
```
```ts
export default defineNuxtConfig({
  modules: [
    '@nuxt/content' // `modules` にNuxt Contentを追加
  ],
  content: {
    // 追加で設定があればここに追加
  }
})
```
```ts
export default defineNuxtConfig({
  modules: [
    '@nuxt/content' // `modules` にNuxt Contentを追加
  ],
  content: {
    highlight: {
      theme: 'github-dark', // たとえばこんなの
    },
  }
})
```
````

---
layout: statement
---

# 記事をMarkdownで書く

---

# ファイル構成

````md magic-move
```bash
/content
  └articles # こうすると `articles/<slug>` で記事を取得できる
    ├── bar.md
    ├── foo.md
    ├── fuga.md
    ├── hoge.md
    └── piyo.md
```
```bash
/content
  └articles # ただし、これだとファイルがslug順になる
    ├── bar.md
    ├── foo.md
    ├── fuga.md
    ├── hoge.md
    └── piyo.md
```
```bash
/content
  └articles # 本当は記事の公開日順でこう並んでほしい
    ├── hoge.md
    ├── fuga.md
    ├── foo.md
    ├── bar.md
    └── piyo.md
```
```bash
/content
  └articles # こうする（日付部分は取得時は無視される）
    ├── 20240101.hoge.md
    ├── 20240102.fuga.md
    ├── 20240103.foo.md
    ├── 20240104.bar.md
    └── 20240105.piyo.md
```
````

---

# カテゴリーやタグ情報はYAMLで定義しておく

<div class="grid grid-cols-2 gap-4">
<div>

## カテゴリー<br>`/content/categories.yml`

```yml
- id: category-1
  name: カテゴリー1
- id: category-2
  name: カテゴリー2
- id: category-3
  name: カテゴリー3
```

</div>
<div>

## タグ<br>`/content/tags.yml`

```yml
- id: tag-1
  name: タグ1
- id: tag-2
  name: タグ2
- id: tag-3
  name: タグ3
```

</div>
</div>

---

# 最終的な `content` の構成

`/content` 配下においたものがNuxt Contentの管理対象になる

```bash
/content
  ├── categories.yml # カテゴリー
  ├── tags.yml # タグ
  └articles # 記事
    ├── 20240101.hoge.md
    ├── 20240102.fuga.md
    ├── 20240103.foo.md
    ├── 20240104.bar.md
    └── 20240105.piyo.md
```

---
layout: statement
---

# 記事を取得する

---

# `queryContent()` による記事の取得

## `find()` 記事一覧の取得

````md magic-move
```ts
// `/content/articles` 以下の記事をすべて取得できる
const articles = await queryContent('articles').find()
```
```ts
// `limit()` で取得件数を制限できる
const articles = await queryContent('articles')
  .limit(5)
  .find()
```
```ts
// `skip()` で取得開始位置を指定できる
const articles = await queryContent('articles')
  .skip(5)
  .find()
```
```ts
// 1ページ10件、2ページ目を取得する場合はこうなる
const articles = await queryContent('articles')
  .skip(10)
  .limit(10)
  .find()
```
````

## 対象範囲

```bash {4-9}
/content
  ├── categories.yml # カテゴリー
  ├── tags.yml # タグ
  └articles # 記事
    ├── 20240101.hoge.md
    ├── 20240102.fuga.md
    ├── 20240103.foo.md
    ├── 20240104.bar.md
    └── 20240105.piyo.md
```

---

# `queryContent()` による記事の取得

## `count()` 記事の件数を取得

````md magic-move
```ts
const count = await queryContent('articles').count()
```
```ts
const count = await queryContent('articles')
  .where({ category: 'category-1' })
  .count()
```
````

## 対象範囲

```bash {4-9}
/content
  ├── categories.yml # カテゴリー
  ├── tags.yml # タグ
  └articles # 記事
    ├── 20240101.hoge.md
    ├── 20240102.fuga.md
    ├── 20240103.foo.md
    ├── 20240104.bar.md
    └── 20240105.piyo.md
```

---

# `queryContent()` による記事の取得

## `findOne()` 記事を1件だけ取得する

```ts
const article = await queryContent(
  `articles/${slug}`
).findOne()
```

## 対象範囲

```bash {4-9}
/content
  ├── categories.yml # カテゴリー
  ├── tags.yml # タグ
  └articles # 記事
    ├── 20240101.hoge.md
    ├── 20240102.fuga.md
    ├── 20240103.foo.md
    ├── 20240104.bar.md
    └── 20240105.piyo.md
```

---

# カテゴリー・タグ情報を取得する

```ts
// YAMLファイルになっても基本は同じ
const articles = await queryContent('categories')
  .findOne();
```

<div class="grid grid-cols-2 gap-4">
<div>

## 対象範囲

```bash {2}
/content
  ├── categories.yml # カテゴリー
  ├── tags.yml # タグ
  └articles # 記事
    ├── 20240101.hoge.md
    ├── 20240102.fuga.md
    ├── 20240103.foo.md
    ├── 20240104.bar.md
    └── 20240105.piyo.md
```

</div>
<div>

## 取得結果

```ts
[
  {
    _path: '/hello',
    _draft: false,
    _partial: false,
    id: 'category-1',
    name: 'カテゴリー1',
    _id: 'content:categories.yml',
    _type: 'yaml',
    _source: 'content',
    _file: 'categories.yml',
    _extension: 'yml'
  },
]
```
</div>
</div>

---
layout: statement
---

# 記事に付加情報を追加する

---

# frontmatter

デフォルトで利用できるfrontmatter

| key | type | default | description |
| --- | --- | --- | --- |
| `title` | `string` | 最初の `<h1>` | 記事のタイトル |
| `description` | `string` | 最初の `<p>` | 記事の概要 |
| `draft` | `boolean` | `false` | 下書きにできる（取得できなくなる） |
| `head` | `object` | true | `<head>` の設定 |

---

# 追加で必要な情報をfrontmatterに追加する

frontmatterはデフォルトの値以外にも自由に追加できる

```md
---
title: 記事のタイトル
category: category-id # 記事のカテゴリー情報を追加
tags: [tag-id-1, tag-id-2] # タグは複数つけられるので配列で指定
ogImage: /images/og-image.png # og:image に使う画像
publishedAt: 2024-04-23 # 記事の公開日（本当はcommitの日時を使いたい）
---
```
<div v-click>
<span >ただし、このままだとデフォルトの項目以外は型が効かない</span>

```ts
const article = await queryContent(`articles/${slug}`).findOne();
      // ここで型が効かない
```
</div>

---

# 対応する型を定義する

これで型が効くようになる

```ts {all|2|5|5-12}
import type {
  MarkdownParsedContent
} from '@nuxt/content/dist/runtime/types';

export interface Article extends MarkdownParsedContent {
  // frontmatter に追加した項目を追加
	category: string;
	tags: string[];
	ogImage: string;
	publishedAt: string;
	updatedAt?: string;
}
```

---

# `queryContent()` に型を追加する

````md magic-move
```ts
const articles = await queryContent('articles')
  .skip(10)
  .limit(10)
  .find()

const article = await queryContent(
  `articles/${slug}`
).findOne()
```
```ts
import type { Article } from '~/types/article';

const articles = await queryContent<Article>('articles')
  .skip(10)
  .limit(10)
  .find()

const article = await queryContent<Article>(
  `articles/${slug}`
).findOne()
```
````

---

# `pages/articles/index.vue` で記事一覧を表示する

```vue
<script lang="ts" setup>

// 重複してfetchしないように `useAsyncData()` でラップする
const { data: articles } = await useAsyncData(
  'articles',
  () => queryContent('articles').find(),
);
</script>
```

---

# `pages/categories/[id].vue` で<br>カテゴリーごとの記事一覧を表示する

````md magic-move
```vue
<script lang="ts" setup>

// 重複してfetchしないように `useAsyncData()` でラップする
const { data: articles } = await useAsyncData(
  'articles',
  () => queryContent('articles').find(),
);
</script>
```
```vue
<script lang="ts" setup>

// 重複してfetchしないように `useAsyncData()` でラップする
const { data: articles } = await useAsyncData(
  'articles',
  () => queryContent('articles')
  // where() でフィルタリングできる
  .where({ category: $route.params.category })
  .find(),
);
</script>
```
```vue
<script lang="ts" setup>

// 重複してfetchしないように `useAsyncData()` でラップする
const { data: articles } = await useAsyncData(
  'articles',
  () => queryContent('articles')
  // これでも同じ意味
  .where({ category: { $eq: $route.params.category }})
  .find(),
);
</script>
```
````

---

# `pages/articles/[...slug].vue` で記事を表示する

```vue
<script lang="ts" setup>
import type { Article } from '~/types/article';

const $route = useRoute();

const { data: article } = await useAsyncData(
  'article',
  () =>  queryContent<Article>($route.fullPath).findOne(),
)
</script>
```

---

# `[...slug].vue` で記事を表示する

```vue
<template>
    <h1>{{ article?.title }}</h1>
    <p>{{ article.category }}</p>
    <ul>
      <li v-for="tag in article.tags" :key="tag">{{ tag }}</li>
    </ul>
    <p>{{ article.description }}</p>
    <p>{{ article.publishedAt }}</p>
    <img :src="article.ogImage" alt="">
    <!-- markdownをレンダリングしてくれる -->
    <content-renderer :value="article" />
</template>
```

---
layout: statement
---

# 完成

---

# まとめ

- Nuxt Contentを使うと<span v-mark="{color: 'red'}">ファイルベースのCMS</span>が簡単に作れる
- <span v-mark="{color: 'red'}">frontmatter</span>を使って記事に付加情報を追加できる
- <span v-mark="{color: 'red'}">`queryContent()`</span> を使えば複雑なクエリも実装できる
- なにより<span v-mark="{color: 'red'}">型定義</span>もしっかりできる

---

# 引用

- [Installation \- Nuxt Content](https://content.nuxt.com/get-started/installation)
- [queryContent\(\) \- Nuxt Content](https://content.nuxt.com/composables/query-content)
- [Markdown \- Nuxt Content](https://content.nuxt.com/usage/markdown)
- [JSON, YAML, CSV \- Nuxt Content](https://content.nuxt.com/usage/files)