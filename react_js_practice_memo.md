# 補足. 学習過程におけるメモ

自分が以前にReact(ReactNative)に触れていた当時からかなり月日が経過してしまっていたこともあり、改めて進めていった際の気づきやメモをこちらに残しています。

### 0. 環境構築に関すること

`create-react-app`が既にインストールされていたが、バージョンが古かったことが原因で新規プロジェクトが作成されませんでした。

※ 下記の様なログが出てしまいました。

```
A template was not provided. This is likely because you're using an outdated version of create-react-app.
Please note that global installs of create-react-app are no longer supported.
```

こちらは古いバージョンのものを消去することで解決ができました。

```shell
# 1. コマンドで削除を試みる
$ npm uninstall -g create-react-app
# 2. あれ、まだ残っている...
$ which create-react-app
/usr/local/bin/create-react-app
# 3. rmコマンドで強制的に削除する
$ rm -rf /usr/local/bin/create-react-app
```

というわけで、再度インストールし直してみましょう。そしてついでにyarnも一緒に入れておきましょう。

```shell
# 1. コマンドで削除を試みる
$ npm install -g create-react-app
# 2. あれ、また失敗した...（あ、権限がないんですね...）
npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules
# 3. まずは権限を付与する
$ sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
# 4. create-react-app & yarnをglobal installする
$ npm install --global yarn
$ npm install --global create-react-app
# ※ 今後このProjectではyarnを利用する想定でいます
```

__【参考】__

+ [npm install -g 実行時に「npm WARN checkPermissions Missing write access to」が発生した場合の対処法](https://mebee.info/2020/03/17/post-5134/)

再度チャレンジしてみましょう。

```shell
$ npx create-react-app story-show-cases --template typescript
```

プロジェクトが生成されたので、次はパッケージ管理ツールの`yarn`を利用するための準備をする。

```shell
# 1. yarnコマンドの実行（※実行後にyarn.lockファイルが生成される）
$ yarn
# 2. ビルドコマンドの実行（※実行後にlocalhost:3000でページが表示される）
$ yarn start
```

そして次にESLint(構文チェック)とPrettier(コード整形)を導入する手順を実行する。

ESLintを導入します。

```shell
# ESLint導入
$ yarn add -D eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

ESLint導入が完了したら、`.eslintrc.js`を新規作成し、まずは下記の記載をします。

```js:.eslintrc.js
module.exports = {
    "plugins": ["prettier"],
    "extends": ["react-app", "prettier"],
    "rules": {
        "prettier/prettier": "error"
    }
};
```

その後にPrettierを導入します。

```shell
# Prettier導入
$ yarn add -D prettier eslint-config-prettier eslint-plugin-prettier
```

Prettier導入が完了したら、`.prettierrc`を新規作成し、まずは下記の記載をします。

```js:.prettierrc
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "arrowParens": "always"
}
```

__【参考】__

+ [create-react-appでReactとTypeScript環境を構築](https://mo-gu-mo-gu.com/create-react-app-typescript/)

