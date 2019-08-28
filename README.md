# Nuxt&Typescriptの環境構築

## nuxtプロジェクト作成

公式を参考にする
https://ja.nuxtjs.org/guide/installation

`eslint`,`prettier`を選択、あとは自由に

その後`package.json`でnuxtのバージョンを2.8.1に書き換える（2019/08/29時点の最新版は2.9.2）

```json
  "dependencies": {
    "nuxt": "2.8.1",
  }
```

## TypeScriptの導入
公式を参考にする
https://ja.nuxtjs.org/guide/typescript

## vscodeのsetting.jsonを変更（これで正しいかは要精査）

`prettier.eslintIntegration`が非推奨と出るので消してあります
https://github.com/prettier/prettier-vscode

```json:setting.json
{
  "prettier.jsxSingleQuote": true,
  "editor.formatOnSave": false, //自動整形が衝突するのでfalseが良い
  "prettier.semi": false,
  "prettier.singleQuote": true,
  // "prettier.eslintIntegration": true,
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

## typescriptに対応したeslint導入

コマンドラインで追加

```
$ yarn add -D @typescript-eslint/parser @typescript-eslint/eslint-plugin
```
`eslint-config-prettier`のバージョンアップ

```javascript:package.json
eslint-config-prettier   ^4.1.0  →   ^6.1.0
```
バージョンアップを反映

```
$ yarn install
```

次に`.eslintrc.js`を変更する、主にeslintがtypescriptに対応するため。
ルールの追加は`no-unused-vars`が余計な箇所にも出てしまうためである。
対策としてこいつを一度オフにしてから`@typescript-eslint`のルールに従ってエラーを出してもらう。

```javascript:.eslintrc.js
module.exports = {
...
  parserOptions: {
    parser: '@typescript-eslint/parser'  //変更
  },
  extends: [
    '@nuxtjs',
    'prettier',
    'prettier/vue',
    'plugin:prettier/recommended',
    'plugin:nuxt/recommended',
    'prettier/@typescript-eslint' //追加
  ],
  plugins: [
    'prettier',
    '@typescript-eslint'  //追加
  ],
  //ルールを2つ追加
  rules: {
    "no-unused-vars": "off",
    "@typescript-eslint/no-unused-vars": "error"
  }
}
```
ただし、このままでは`nuxt.confg.ts`で`@typescript-eslint/no-unused-vars`エラーになる箇所があるので修正する。

```typescript:nuxt.confg.ts
const config: NuxtConfiguration = {

...

  build: {
    extend(config, ctx) {
      if (ctx.isDev && ctx.isClient) {
        if (!config.module) return // undefinedの場合、pushせずにreturnするように追加
        config.module.rules.push({
          enforce: 'pre',
          test: /\.(js|vue)$/,
          loader: 'eslint-loader',
          exclude: /(node_modules)/
        })
      }
    }
  }
}

```

`package.json`の`lint`コマンドに`.ts`を追加する

```json:package.json
  "scripts": {
    "lint": "eslint --ext .ts,.js,.vue --ignore-path .gitignore ."
  },

```
これでtsファイルもlintしてくれるようになる

## Vue Property Decoratorのインストール

コマンドラインで追加する

```
$ yarn add vue-property-decorator
```

細かい書き方はGithubを参考にする
https://github.com/kaorun343/vue-property-decorator

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
