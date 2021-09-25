---
title: Next入門
description: Nextを利用した基本的な開発方法を解説しています
---

[Nuxtjs](https://ja.nuxtjs.org/)

## プロジェクトの作成

### 新しい npm が入っていると以下のコマンドが使える

初回は「create-nuxt-app」がインストールされる

>npx create-nuxt-app <project-name>
>npm run dev

## firebase

>npm install firebase
>npm install @nuxtjs/firebase

## 追加プラグイン

>npm install browser-image-compression --save

[Nuxt Firebase](https://firebase.nuxtjs.org/guide/getting-started)

## ディレクトリー構成

```
root
|-api（追加）
|--Repositories（追加）
|-pages
|-components
|-assets
|--images（追加）
|-static
|-nuxt.config.js
|-package.json
|-content
|-layouts
|-middleware
|-modules(追加)
|-plugins
|--repositoryFactory.js（追加）
|--common.js（追加）
|-store
```

## routing

- ルーティングはフォルダーとページを配置することで自動生成してくれる。
- _id.vue などを作ると、id をパラメータとしたルーティングをしてくれる

パラメータのチェックは validate メソッド」を追加することでチェックができる（return で turue、false を返すことで、チェックエラー時はページが見つかりませんのエラーにできる

## view

### layout

- デフォルトでは.next/views/app.template.html が利用される。
- カスタマイズする場合は、ルートに app.html を作成するとこれが利用される
- 階層

  1. root/app.html
  1. layouts/default.vue
  1. 個別に適用されるレイアウト

  ```
  |- app.html
  |--{{ APP }}
  |- default.vue
  |--<nuxt />
  |-各ページ
  ```

### error ページ

- layouts/error.vue
- エラーページをカスタマイズするのは layouts/error.vue を作成する
- パラメータでえらーコードを取得することもできる
  ```
  Props['error']
  ```

### error ページ

HTML ヘッダーを変えたいときは「nuxt.config.js」を変更する

- フォントの追加など

## 非同期通信

- axios を使う

## assets

画像などの静的ファイルを配置する
```
img="~/assets/images/XXX,jpg"
```

## 静的なファイル

- robots.txt
- sitemap
- favicon

## modules

1. ルートに modules フォルダーを作成する

## 状態管理（vuex）

アプリケーションの状態（State）を管理する拡張機能。

複数のコンポーネント間でデータの動機を管理する（アプリケーションの状態を 1 っか所で管理する）

- 直接値を更新しない（必ず Mutation を経由して変更する）
- 非同期処理で更新したい場合は Action を経由する（必須ではない）

Store
データ保管場所

- Store ディレクトリに「index.js」を作成する
- vuex をインポートする
- ストアーを作成して Export する
- State を定義する

### アクセス方法

```js
#store.state.値
```

### 値の更新

```js
mutations:{
    updateXXX:function (state,msg){
        state.msg = msg;
    }
}
```

```js
$store.commit('ミューテーション', 渡したい値)
```

### Action の利用

```js
actions:{
    updateXXX(context,msg){
        context.commit('updateXXX',msg)
    }
}
```

```js
$store.dispatch('Action',渡したい値))
```

## モジュールモード

- クラシックモード
  1 つのファイルで管理
- モジュールモード
  複数のモジュールで管理

## DarkMode の設定の変更

```js
vuetify: {
customVariables: ['~/assets/variables.scss'],
theme: {
dark: false,
themes: {
dark: {
primary: colors.blue.darken2,
accent: colors.grey.darken3,
secondary: colors.amber.darken3,
info: colors.teal.lighten1,
warning: colors.amber.base,
error: colors.deepOrange.accent4,
success: colors.green.accent3,
},
},
},
},
```

## 開発手順

1.  プロジェクトの作成

    ### プロジェクト設定

        - あああ
        - あああ

    ### プラグイン

        - あああ
        - あああ

1.  State の作成

    モジュールコンポーネントを作成

    ### コードテンプレート

    ```js
    export ()=>{

    }

    ```

1.  コンポーネントの作成

    ### コードテンプレート

    ```js
    export ()=>{

    }

    ```

1.  View の作成

    ### コードテンプレート

    ```js
    export ()=>{

    }
    ```

## 独自の loadting 画面を利用する

[ローディング](https://ja.nuxtjs.org/docs/2.x/features/loading)

1. コンポーネントフォルダー配下に「LoadingBar.vue」を作成
a
```js
<template>
  <div>
    <div v-if="loading" class="loading-page">
      <p>Loading...</p>
    </div>
  </div>
</template>

<style scoped>
.loading-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(240, 232, 232, 0.8);
  text-align: center;
  padding-top: 200px;
  font-size: 30px;
  font-family: sans-serif;
  opacity: 0.6 ;
  /* AppBarや画面要素が利用できないように、最前面に表示する */
  z-index: 100;
}
</style>

<script>
export default {
  data: () => ({
    loading: false,
  }),
  methods: {
    start() {
      this.loading = true
    },
    finish() {
      this.loading = false
    },
  },
}
</script>

```

1. Config に独自の Loading 画面を指定する

   ```js
   loading: '~/components/LoadingBar.vue'
   ```

1. Loading 画面を表示する
   nuxt が自動で呼び出す部分もこれが利用されます。アプリケーションから個別に利用する場合は以下のように、Loading の表示非表示を制御します。

   Loading を表示

   ```js
   this.$nuxt.$loading.start()
   ```

   Loadhing を非表示

   ```js
   this.$nuxt.$loading.finish()
   ```

   画面の Mount で一定時間表示させる例

   ```js
     methods: {
       start() {
         this.$nuxt.$loading.start()
         setTimeout(() => this.$nuxt.$loading.finish(), 5000)
       },
     },
     mounted() {
       console.log('aa')
       this.$nextTick(() => {
         this.start()
       })
     },
   ```

## firebase

1. firebase プロジェクトの作成

1. CloudFireStore
   ※テストモードはプロジェクト ID がわかるとアクセスできるので注意（認証なしでアクセスできる）

1. Command
>npm install --save firebase
>npm install --save vuexfire

[VuexFire で Nuxt.js アプリに一瞬で Firestore を導入する](https://qiita.com/wataruoguchi/items/8fdda8e9754658be06be)

Vuex の Mutation でデータの変更先を Firestore にて、Firestore 経由で Vuex の Store を扱う

1. 環境変数の設定
   > npm install --save @nuxt/dotenv

- プロジェクトのルートに「.env」ファイルを作成
  しする
- gitignore で除外の設定をする
  -nuxt.config にもジュールを読み込む設定をする

1. pulugin ないに firebase.js を作成
   し firebase を初期化して export する

```js
import firebase from 'firebase'

const config = {
  databseURL: process.env.FIREBASE_DATABASE_URL,
  projectId: process.env.FIREBASE_PROJECT_ID,
}

if (!firebase.apps.length) {
  firebase.initializeApp(config)
}

export default firebase
```

1. store を定義する

```js
import { vuexfireMutations } from 'vuexfire'

export const mutations = {
  ...vuexfireMutations,
}
```

## vuetify のテーマを変更する

追加して使うこともできます

nuxt.config.js

```js
  vuetify: {
    customVariables: ['~/assets/variables.scss'],
    theme: {
      dark: true,
      themes: {
        dark: {
          primary: colors.blue.darken2,
          accent: colors.grey.darken3,
          secondary: colors.amber.darken3,
          info: colors.teal.lighten1,
          warning: colors.amber.base,
          error: colors.deepOrange.accent4,
          success: colors.green.accent3,
          background: '#000000',
        },
        /*以下追加*/
        light: {
          primary: colors.blue.darken2,
          accent: colors.grey.darken3,
          secondary: colors.amber.darken3,
          info: colors.teal.lighten1,
          warning: colors.amber.base,
          error: colors.deepOrange.accent4,
          success: colors.green.accent3,
          background: '#FFFFFF',
        },
      },
    },
  },
```

使い方

```js
  <v-app-bar fixed flat app style="height: 48px" color="background">

```

テーマを切り替える方法
>this.$vuetify.theme.dark = this.switch1;

## ページ毎に Layout ファイルを変更する方法
