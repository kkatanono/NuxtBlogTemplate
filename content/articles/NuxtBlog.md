---
title: NuxtでBlogを作成する方法
description: Nuxt/Contentを利用したBlogの作成手順を解説しています。
---

# Nuxt Blog Template

## 概要
NextをベースにSSGでブログページなど静的サイトをデプロイする方法です。利用するモジュールはnuxtのcontentを利用し、MarkDownファイルをビルド時にAPIで取得し、静的サイトを生成します。

静的サイトとなるのでコンテンツの追加、変更はビルド、デプロイとなりますので、ホームページやブログなどの用途に向いています。

1. コンテンツの作成⇒ビルド⇒デプロイ
1. コンテンツの変更・追加⇒ビルド⇒デプロイ


## nuxt/content

nuxt/contentの公式ページは以下となります。

[nuxt/content](https://content.nuxtjs.org/ja)

## Project

最初にNuxtプロジェクトを作成します。
ポイントは、SSGとJamstackを選択することです。

- Rendering mode Universal (SSR / SSG)
- Deployment target Static (Static/Jamstack hosting)

また、Contentを利用しますので、Nuxt.js modulesでまた、Contentを利用しますので、Nuxtを選択しておきます。

### プロジェクトの構成
>npx create-nuxt-app NuxtBlogTemplate

```
Generating Nuxt.js project in NuxtBlogTemplate
? Project name: NuxtBlogTemplate
? Programming language: JavaScript
? Package manager: Npm
? UI framework: Vuetify.js
? Nuxt.js modules: Axios - Promise based HTTP client, Progressive Web App (PWA), Content - Git-based headless CMS
? Linting tools: Prettier
? Testing framework: None
? Rendering mode: Universal (SSR / SSG)
? Deployment target: Static (Static/Jamstack hosting)
? Development tools: (Press <space> to select, <a> to toggle all, <i> to invert selection)
? Continuous integration: None
? Version control system: Git
```

## コンテンツの作成
プロジェクトを作成するとルートにcontentフォルダーが作成されています。ここにMarkdown、JSON、YAML、XML、CSVファイルを入れることで、APIを使ってデータを取得するヘッドレスCMSを実現しています。

WordPressのMongoDBに格納する情報をこのフォルダ―に配置することCMSを実現するイメージです。（あくまでもイメージです）

1. contentフォルダーにマークダウンファイルを配置します。

    content/hello.mdに初期ファイルが作成していますが、articlesフォルダ―を作成し、その中にMarkdownファイルを配置します

    ```
    content
    |-articles
    |--articles1.md
    |--articles2.md
    ```

2. 記事の作成

    Markdownファイルには、titleなどSEOのためのmeta情報を含めることができます。

    >articles1.md
    ```
    ---
    title: Introduction
    description: Learn how to use @nuxt/content.
    ---
    ```

# コンテンツの表示

作成したMarkdownファイルは以下のようなAPIを経由してアクセスできます。
```js
const content = await context.$content('articles/articles1').fetch()
```
取得したコンテンツは、meta情報にもアクセスできます。Markdownの本文をHTMｌに展開して表示するためのコンポーネント（nuxt-content）を利用することで本文の表示が可能です。

```html
<h1>{{ content.title }}</h1>
<nuxt-content :document="content" />
```
## 動的ルーティング

ページを表示するためには、/Pagesに上記のコンテンツを取得するAPIを組み込んだページを作成する必要があります。これを実現するために

「content/articles/articles1.md」を表示するために「http://localhost:3000/articles/articles1」でアクセスできると良いのですが、自動では作成されません。

対応する個々のページを作るのでは、効率が悪いので、以下の動作をづるページを作成します

1. articles/articles1のURLばあい、articles1をパラメータとするページを作成
1. articles1のコンテンツをAPIで取得
1. articles1の本文を表示

```
pages/
|-article/
|--_slug.vue
|--index.vue

```
- _slug.vue：パラメータで指定されたコンテンツの内容を表示するページ

_slug のようにアンダースコアではじまるファイル名は動的ルーティングのスラッグを格納する変数名となり、 params.slug のように参照する事ができます。

```vue
<template>
 <article>
   <h1>{{blogs.title}}</h1>
   <p>{{blogs.date}}</p>
   <nuxt-content :document="blogs" />
 </article>
</template>
<script>
export default {
 async asyncData ({ $content, params }) {
   const blogs = await $content('articles', params.slug || 'index').fetch()
   return { blogs }
 }
}
</script>
```

- index.vue：コンテンツの一覧ページ

ここでは上位１０件の一覧を表示しています。Sort、絞り込みなども実現可能です。

```vue
<template>
 <div>
   <div v-for="b in blogs" :key="b.slug">
     <nuxt-link :to="'/articles/'+ b.slug">{{b.title}} {{b.date}}</nuxt-link>
   </div>
 </div>
</template>
<script>
export default {
 async asyncData ({ $content, params }) {
   const query = await $content('articles' || 'index').limit(10)
   const blogs = await query.fetch()
   return { blogs }
 }
}
</script>
```
## generate

デフォルトのままgenerateだけでは、静的ファイルは生成されません。nuxt.config.js に generate の設定を行います。

```js
  generate: {
    async routes() {
      const { $content } = require('@nuxt/content')
      const files = await $content({ deep: true }).only(['path']).fetch()
      return files.map((file) => (file.path === '/index' ? '/' : file.path))
    },
  },
```

>npm run generate

generateを実行すると、/distフォルダ―にコンテンツ毎のフォルダーが作成され、各ContentsのIndexファイルを作成されています。これをvercelなどにデプロイすることで完成です。


>npm run start 

でdistを表示指せることができます。

## デプロイ
主なデプロイ先

Firebase hosting
AZURE Static Web App
vercel
Netlify
GitHub Action


## 参考資料
[Nuxt Contentで静的サイトの初期設定をする話](https://note.matsukaze.design/article/2021/0130-nuxt-content)