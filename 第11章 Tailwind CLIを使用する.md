# 11. Tailwind CLIを使用する

## 11.1 Tailwind CLIの特徴

### インストールが不要

```js
// カスタマイズ用のtailwind.cssファイルの例
/* ./src/tailwind.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .btn {
    @apply px-4 py-2 bg-blue-600 text-white rounded;
  }
}
```

```html
<!-- 生成したTailwind CSSのCSSファイルを利用する例 -->
<!doctype html>
<html>
<head>
  <!-- ... -->
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link href="./dist/tailwind.css" rel="stylesheet">
</head>
<body>
  <!-- ... -->
</body>
</html>
```

### PostCSSプラグインのサポート

```js
// PostCSSプラグインを追加する例
// postcss.config.js（プロジェクトのルートに置く）
module.exports = {
  plugins: [
    require('postcss-100vh-fix'),
  ]
}
```

```js
// CSSを生成する前に実行するPostCSSプラグインを追加する例
// postcss.config.js
module.exports = {
  plugins: [
    require('postcss-import'),
    require('tailwindcss'),
    require('postcss-100vh-fix'),
  ]
}
```
