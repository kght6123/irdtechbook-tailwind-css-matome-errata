
# 10. Just-in-Time Modeを使用する

## 10.1 Just-in-Time Modeのメリット

```
/* 角括弧表記でユーティリティを生成する */
top-[-113px]
```

```
/* 角括弧表記でユーティリティを生成し、全てのバリアントも使用できる。 */
md:top-[-113px]
```

## 10.3 Just-in-Time Modeの有効化

```js
// Just-In-Time Modeを有効にする
  module.exports = {
+   mode: 'jit',
+   purge: [
+     './public/**/*.html',
+     './src/**/*.{js,jsx,ts,tsx,vue}',
+   ],
    theme: {
      // ...
    }
    // ...
  }
```

## 10.4 Just-in-Time Modeの新機能について

### 任意の値（v2.1追加、v2.2更新）

```html
<!-- 使用できる任意の値の例 -->
<!— 横幅、縦幅、位置に対して、任意の値を指定できる —>
<img class="absolute w-[762px] h-[918px] top-[-325px] right-[62px] md:top-[-400px] md:right-[80px]" src="/crazy-background-image.png">

<!— 色に対して、任意の値を指定できる —>
<button class="bg-[#1da1f1]">Share on Twitter</button>

<!— グリットの列や行の定義に対して、任意の値を指定できる —>
<div class="grid-cols-[1fr,700px,2fr]">
  <!— … —>
</div>
```

```html
<!-- 間にスペースがあるので動作しない例 -->
<div class="h-[calc(1000px - 4rem)">…</div>
<div class="grid-cols-[1fr 700px 2fr]">…</div>
```

```html
<!-- 間のスペースを削除したり、カンマに置き換えると動作する例 -->
<div class="h-[calc(1000px-4rem)]">…</div>
<div class="grid-cols-[1fr,700px,2fr]">…</div>
```

```html
<!-- 長さや色、角度を任意の値で指定する -->
<div class="col-start-[73] placeholder-[#aabbcc] object-[50%] ..."></div>
```

```html
<!-- テキストのフォントサイズと色を指定した場合 -->
<div class="text-[length:var(--my-fontsize-var)]">あああ</div>
<div class="text-[color:var(--my-color-var)]">あああ</div>
```

```css
/* CSS変数の指定 */
:root {
--my-color-var: #aa00cc;
--my-fontsize-var: 100px;
}
```

### important修飾子（v2.1追加）

```html
<!-- important修飾子の記述例 -->
<p class="font-bold !font-medium">
  吾輩は猫である。名前はまだ無い。どこで生れたかとんと見当がつかぬ。
何でも薄暗いじめじめした所でニャーニャー泣いていた事だけは記憶している。吾輩
</p>
<div class="sm:hover:!tw-font-bold">
```

### 最初または最後の子要素の擬似要素（before、after）のサポート（v2.2追加）

```html
<!-- before、after擬似要素の記述例 -->
<div class="before:block before:bg-blue-500 after:flex after:bg-pink-300"></div>
```

```html
<!-- content に’hello’を指定した記述例 -->
<div class="before:content-['hello'] before:block ..."></div>
```

```html
<!-- contentにbefore属性の値を指定した例 -->
<div before="hello world" class="before:content-[attr(before)] before:block ..."></div>
```

### 先頭文字/行の擬似要素（first-letter/line）のサポート（v2.2追加）

```html
<!-- 先頭文字のみ文字サイズを大きくした例 -->
<p class="first-letter:text-4xl first-letter:font-bold first-letter:float-left">
木曾路はすべて山の中である。
あるところは岨づたいに行く崖の道であり、あるところは数十間の深さに臨む
</p>
```

### 選択した文章の擬似要素（selection）のサポート（v2.2追加）

```html
<!-- 選択した文字の背景色をピンクにした例 -->
<p class="selection:bg-pink-200">
木曾路はすべて山の中である。
あるところは岨づたいに行く崖の道であり、あるところは数十間の深さに臨む
</p>
```

### リスト項目の箇条書き記号ボックスの擬似要素（marker）のサポート（v2.2追加）

```html
<!-- 番号付きリスト項目を大きめにした例 -->
<h1>WrestleMania XII Results</h1>
<ol role="list" class="list-decimal list-inside marker:text-gray-500 marker:font-medium marker:text-3xl">
  <li>日本国民は、正当に選挙された国会における代表者を通じて行動し、</li>
  <li>われらとわれらの子孫のために、諸国民との協和による成果と、</li>
  <li>わが国全土にわたつて自由のもたらす恵沢を確保し、</li>
  <li>政府の行為によつて再び戦争の惨禍が起ることのないやうにすることを決意し、</li>
  <li>ここに主権が国民に存することを宣言し、この憲法を確定する。</li>
  <li>そもそも国政は、国民の厳粛な信託によるものであつて、</li>
</ol>
```

### 兄弟要素を選択するバリアント（peer）（v2.2追加）

```html
<!-- チェックボックスにチェックを入れると、spanのスタイルも変わる例 -->
<label>
  <input type="checkbox" class="sr-only peer">
  <span class="w-4 h-4 bg-gray-100 peer-checked:bg-blue-500 peer-checked:text-blue-50">チェックボックスだよ！</span>
</label>
```

### 擬似クラスの追加（v2.2追加）

```html
<!-- placeholder-shownを使用した例 -->
<div class="relative">
  <input id="name" class="peer" placeholder="placeholder" />
  <label for="name" class="peer-placeholder-shown:top-0 peer-focus:top-[-1.75rem] absolute block">placeholder</label>
</div>
```

### 透明度と色の同時指定（v2.2追加）

```html
<!-- 透明度と色の同時指定を使用しない例 -->
<div class="bg-red-500 bg-opacity-25">
```

```html
<!-- 透明度と色の同時指定を使用する例 -->
<div class="bg-red-500/25">
```

```html
<!-- グラデーションの色など、色全般に使用できます -->
<div class="bg-gradient-to-r from-red-500/50"></div>
```

```html
<!-- 透明度には小数点も使用できます -->
<div class="bg-red-500/[0.31]"></div>
```

### カーソルの色を指定（v2.2追加）

```html
<!-- inputタグのカーソルの色を指定する -->
<input class="caret-red-500" />
```

### transformとfilter指定の簡略化（v2.2追加）

```html
<!-- transformとbackdrop-filterの簡略化を使用しない例 -->
<div class="transform scale-50 filter grayscale backdrop-filter backdrop-blur-sm">
```

```html
<!-- transformとbackdrop-filterの簡略化を使用する例 -->
<div class="scale-50 grayscale backdrop-blur-sm">
```

```html
<!-- transformの簡略化を使用しない例（hoverしたときのみ変形する） -->
<div class="scale-105 -translate-y-1 hover:transform">
```

```html
<!-- transformの簡略化を使用する例（hoverしたときのみ変形する） -->
<div class="hover:scale-105 hover:-translate-y-1">
```

### 片側の境界線の色を指定（v2.2追加）

```html
<!-- 片側の境界線の色の指定を使用した例 -->
<div class="border-2 border-t-blue-500 border-r-pink-500 border-b-green-500 border-l-yellow-500">
  <!-- ... -->
</div>
```

### セーフリストに指定して、purgeされないようにする（v2.2追加）

```js
// 組み込みのセーフリストを指定する例
// tailwind.config.js
module.exports = {
  purge: {
    content: ['./src/**/*.html'],
    safelist: [
      'bg-blue-500',
      'text-center',
      'hover:opacity-100',
      // ...
      'lg:text-right',
    ],
  },
  // ...
}
```

### コンテンツを変換して、クラス名を検出する方法（v2.2追加）

```js
// *.mdファイルを、remarkでhtmlに変換する例
// tailwind.config.js
let remark = require('remark')

module.exports = {
  purge: {
    content: ['./src/**/*.{html,md}'],
    transform: {
      md: (content) => {
        return remark().process(content)
      },
    },
  },
  // ...
}
```

### カスタマイズして、クラス名を検出させる方法（v2.2追加）

```js
// *.pug ファイルのクラス名の検出ロジックをカスタマイズする例
// tailwind.config.js
module.exports = {
  purge: {
    content: ['./src/**/*.pug'],
    extract: {
      pug: (content) => {
        return /[^<>"'`\s]*/.match(content)
      },
    },
  },
  // ...
}
```
