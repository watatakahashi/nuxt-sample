# Nuxt&Typescriptの環境構築

## nuxtプロジェクト作成

公式を参考にする
https://ja.nuxtjs.org/guide/installation

eslint,prettierを選択、あとは自由に

その後package.jsonでnuxtのバージョンを2.8.1に書き換える

## TypeScriptの導入
公式を参考にする
https://ja.nuxtjs.org/guide/typescript

## vscodeのsetting.jsonを変更（これで正しいかは要精査）

```javascript:setting.json
{
  "prettier.jsxSingleQuote": true,
  "editor.formatOnSave": false, //自動整形が衝突するのでfalseが良い
  "prettier.semi": false,
  "prettier.singleQuote": true,
  // "prettier.eslintIntegration": true, //非推奨と出るため
  "eslint.autoFixOnSave": true,
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "typescript",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  "vetur.format.defaultFormatter.html": "prettier"
}
```

## Vue-property-decoratorのインストール

コマンドラインで追加する

```
$ yarn add vue-property-decorator
```

細かい書き方はGithubを参考にする
https://github.com/kaorun343/vue-property-decorator

## その他

- Package.jsonの[lint]コマンドに.tsを追加
    - Tsファイルもlintしてくれるようになる
- .eslintrc.jsにparserオプションを追加する

```javascript:.eslintrc.js
module.exports = {
...
  parserOptions: {
    parser: '@typescript-eslint/parser'
  },
...
}
```

## 現状の困りごと

### nuxt.config.tsでno-unused-varsが出る

```typescript:nuxt.config.ts
import NuxtConfiguration from '@nuxt/config'

const config: NuxtConfiguration = {
```

使ってるんだが。。。
<img width="605" alt="スクリーンショット 2019-08-28 20.09.03.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/305510/667546b7-f1d7-c3a0-8674-a46e5ecc15c6.png">


# nuxt-sample

> My wondrous Nuxt.js project

## Build Setup

``` bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```
