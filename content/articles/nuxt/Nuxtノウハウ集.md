---
title: Nuxtノウハウ集
description: Nuxtで開発を行う時に、よく利用する機能の実現方法をTips形式で解説しています
---

# Nuxtのノウハウ集

## ページを日本語に設定する方法
nuxt.config.js
```js
  head: {
    titleTemplate: '%s - HabitOfThinking',
    title: 'HabitOfThinking',
    htmlAttrs:{
      lang: 'ja'
    },
        meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: '' },
    ],
    link: [{ rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }],
  },
```

<template lang="pug"> h1.red Hello {{ name }}! </template>


## 共通のコンポーネントを作成する

確認ダイアログやスナックバーはいろいろなモジュールで利用します。共通で簡単に利用できるようにする方法です
[Nuxt.jsにて特定のコンポーネントを全てのページで使えるようにする](https://crieit.net/posts/Nuxt-js)
1. 共通コンポーネントを作成する
componentsフォルダーに「ComfirmDialog.vue」を作成します
```js
<template>
  <v-dialog max-width="300" v-model="show">
    <v-card>
      <v-toolbar color="warning" dark>Warning</v-toolbar>
      <v-card-text>
        <p>{{ message }}</p>
      </v-card-text>
      <v-card-actions class="justify-end">
        <v-btn @click="ok">OK</v-btn>
        <v-btn @click="cancel">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  data() {
    return {
      show: false,
      message: '',
      title: '',
      resolve: null,
      reject: null,
    }
  },
  props: {},
  created() {},
  methods: {
    open(title, message, options) {
      this.show = true
      this.message = message
      this.title = title
      //   this.options = Object.assign(this.options, options)
      return new Promise((resolve, reject) => {
        this.resolve = resolve
        this.reject = reject
      })
    },
    ok() {
      this.resolve(true)
      this.show = false
    },
    cancel() {
      this.resolve(false)
      this.show = false
    },
  },
}
</script>

```

1. pluginsフォルダーに「common.js」を作成します
```js

import Vue from 'vue'

import ComfirmDialog from '~/components/Dialog/ComfirmDialog.vue'
import MessageSnackbar from '~/components/Dialog/MessageSnackbar.vue'

Vue.component('ComfirmDialog', ComfirmDialog)
Vue.component('MessageSnackbar', MessageSnackbar)

```

1. 「nuxt.config.js」に以下の定義を追加します


```js
 plugins: ['~plugins/common.js'],
 ```

 1. 利用方法
Inportとコンポーネントの定義なしで、テンプレートに記述するだけで利用できるようになります。


 ```js
<template>
     <ComfirmDialog ref="confirm" />
</template>

<script>
export default {
  name: 'AppBar',
  data: () => ({}),
  components: {},
  methods: {
    async saveData() {
      if (await this.$refs.confirm.open('Save', 'Are you sure?', { color: 'red' })) {
        console.log('--yes')
      } else {
        console.log('--no')
      }
    },
  },
}
</script>
 ```

 ## ページ毎にレイアウトを変更する

[Nuxtでページごとにテンプレートを切り替えたいとき](https://qiita.com/titaniumkun/items/64d8d73d712e2cb787cd)

1. layoutsフォルダーに新しいレイアウトを作成します「blank.vue」
```js
<template>
  <v-app dark>
    
    <!-- メインコンテンツ -->
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>

export default {
  components: {
  },
  data() {
    return {
    }
  },
}
</script>

```
1. 利用するページで使用するレイアウトを指定します。
```js
 <template>
    <h1>aaa</h1> 
 </template>

<script>
export default {
  layout: 'blank',
  data() {
    return {};
  },
};
</script>


## Link
```js
          <router-link :to="item.to">{{ item.title }}</router-link>
```

## 認証済みの場合のみ
[middleware](https://ja.nuxtjs.org/docs/2.x/directory-structure/middleware/)
[nuxt.js+Firebaseで認証ページを作る](https://inadati.hatenablog.com/entry/2019/10/01/213356)

## ローディング画面
[](https://ja.nuxtjs.org/docs/2.x/features/loading/)