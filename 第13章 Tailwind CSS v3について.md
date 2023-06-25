# 13. Tailwind CSS v3について

## 13.2 変更点

### tailwind.config.jsのpurgeがcontentに変わります（alpha.1）

```js
// v2 の場合
// tailwind.config.js
module.exports = {
  purge: {
    enabled: true,
    content: ['./src/**/*.html' /* ... */],
    safelist: [
      'bg-blue-500',
      'text-center',
      'hover:opacity-100',
      'lg:text-right',
    ],
    transform: {
      md: (content) => {
        return remark().process(content)
      }
    },
    extract: {
      md: (content) => {
        return content.match(/[^<>"'`\s]*/)
      }
    }
  },
  // ...
}
```

```js
// v3 の場合
// tailwind.config.js
module.exports = {
  // purge を content に変更
  content: {
    // purge 内の content は files へ変更
    files: ['./src/**/*.html' /* ... */],
    transform: {
      md: (content) => {
        return remark().process(content)
      },
    },
    extract: {
      md: (content) => {
        return content.match(/[^<>"'`\s]*/)
      },
    },
  },
  // safelist は、直下へ移動
  safelist: [
    'font-bold',
    'text-center',
    // safelist の除外するリストに正規表現やバリアントを指定することができます
    { pattern: /bg-(red|blue|green)/, variants: ['hover', 'focus'] },
  ],
}
```

## 13.3 新規機能

### Play CDNの提供（alpha.1）

```html
<!-- Play CDNの使い方 -->
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Play CDN のサンプル</title>
    <!-- Tailwind CSS のみ使用する場合 -->
    <script src="https://cdn.tailwindcss.com/"></script>
    <!-- Tailwind CSS + 公式プラグインを使用する場合 -->
    <!-- <script src="https://cdn.tailwindcss.com/?plugins=forms,typography,aspect-ratio,line-clamp"></script> -->
    <!-- Tailwind CSS の設定をカスタマイズしたい場合に、下記を追加 -->
    <script>
      // ここに tailwind.config.js に記述する内容を書く
      tailwind.config = {
        theme: {
          extend: {
            colors: {
              tomato: 'tomato',
            },
          },
        },
      }
    </script>
    <!-- Tailwind CSS の機能を使って、カスタム CSS を追加したい場合に、下記を追加 -->
    <style type="text/tailwindcss">
      /* ここに Tailwind CSS のカスタム関数や命令を利用した CSS を記述できる */
      body {
        @apply bg-pink-500;
      }
    </style>
  </head>
  <body>
    <!-- ここに Tailwind CSS を使った HTML が記述できる -->
  </body>
</html>
```

### aspect-ratioユーティリティが追加されます（alpha.1）

```html
<!-- 任意の値を指定する -->
<img class="aspect-[16/10]"/>
<img class="aspect-[16/9]"/>
<img class="aspect-[4/3]"/>
```

### scroll-snapユーティリティが追加されました（alpha.1）

```html
<!-- スクロールすると次のページを見せるデザインのサンプルコード -->
<div class="space-y-4 p-4">
  <!-- X方向にスクロールすると、中央にスナップする -->
  <div class="snap-x snap-proximity w-96 aspect-[2/1] flex overflow-auto">
    <div class="snap-center bg-red-500 aspect-[2/1] text-center">a</div>
    <div class="snap-center bg-yellow-500 aspect-[2/1] text-center">b</div>
    <div class="snap-center bg-blue-500 aspect-[2/1] text-center">c</div>
  </div>
  <!-- Y方向にスクロールすると、中央に必ずスナップしスナップ位置を通りすぎない -->
  <div class="snap-y snap-mandatory w-48 aspect-[1/1.25] overflow-auto">
    <div class="snap-center snap-always bg-red-500 aspect-[1/1.25] text-center">a</div>
    <div class="snap-center snap-always bg-yellow-500 aspect-[1/1.25] text-center">b</div>
    <div class="snap-center snap-always bg-blue-500 aspect-[1/1.25] text-center">c</div>
  </div>
  <!-- X、またはY方向にスクロールすると、中央に必ずスナップする -->
  <div class="snap-both snap-mandatory w-48 aspect-square overflow-auto">
    <div class="grid grid-rows-3 grid-cols-3 w-[36rem] aspect-square">
      <div class="snap-start bg-red-500 aspect-square text-center">a</div>
      <div class="snap-start bg-yellow-500 aspect-square text-center">b</div>
      <div class="snap-start bg-blue-500 aspect-square text-center">c</div>
      <div class="snap-start bg-pink-500 aspect-square text-center">a</div>
      <div class="snap-start bg-violet-500 aspect-square text-center">b</div>
      <div class="snap-start bg-purple-500 aspect-square text-center">c</div>
      <div class="snap-start bg-green-500 aspect-square text-center">a</div>
      <div class="snap-start bg-orange-500 aspect-square text-center">b</div>
      <div class="snap-start bg-gray-500 aspect-square text-center">c</div>
    </div>
  </div>
</div>
```

### scroll-behaviorユーティリティが追加されました（alpha.1）

```html
<!-- 同じページ内のリンクへスクロールする際の動きを指定するサンプルコード -->
<!-- ※ 注意、Tailwind Play でこのサンプルコードは動作しません。 -->
<div class="space-y-4 p-4">
  <nav>
    <a href="#page-1">page1</a>
    <a href="#page-2">page2</a>
    <a href="#page-3">page3</a>
  </nav>
  <section class="snap-y snap-mandatory w-48 aspect-[1/1] overflow-auto">
    <article id="page-1" class="snap-center snap-always bg-red-500 aspect-[1/1] text-center">1</article>
    <article id="page-2" class="snap-center snap-always bg-yellow-500 aspect-[1/1] text-center">2</article>
    <article id="page-3" class="snap-center snap-always bg-blue-500 aspect-[1/1] text-center">3</article>
  </section>
</div>
```

### text-indentユーティリティが追加されました（alpha.1）

```html
<!-- 字下げ幅を指定するサンプルコード -->
<div class="space-y-4 p-8">
  <section class="w-48">
    <article class="bg-red-200 indent-4">これは偶然単にこの所有方ってのの中が思いななけれ。</article>
    <article class="bg-yellow-200 -indent-4">よくほかをお話者ももっともその説明たないまでを行き届いばしまうたでは運動申したずと、それほどには向いたないまします。</article>
    <article class="bg-blue-200 indent-[50%]">形をしですはずもまあ昔からもしでたでし。</article>
  </section>
</div>
```

### columnsユーティリティが追加されました（alpha.1）

```html
<!-- columnsを使用するサンプルコード -->
<div class="space-y-4 p-4">
  <section class="w-full">
    <!-- 数値（段数）を指定すると、横幅に関係なく、2段になる -->
    <article class="bg-red-200 indent-4 columns-2">
      <span class="font-black">（段数を指定）</span>
      親譲りの無鉄砲で小供の時から損ばかりしている。小学校に居る時分学校の二階から飛び降りて
      一週間ほど腰を抜かした事がある。なぜそんな無闇をしたと聞く人があるかも知れぬ。
      別段深い理由でもない。
    </article>
    <!-- 横幅を指定すると、要素の横幅の大きさに応じて段数が増える（下記の場合、横幅 ÷ 3xs（16rem）＝ 段数（切り捨て））-->
    <article class="bg-yellow-200 indent-4 columns-3xs">
      <span class="font-black">（横幅を指定）</span>
      新築の二階から首を出していたら、同級生の一人が冗談に、いくら威張っても、そこから飛び降
      りる事は出来まい。弱虫やーい。と囃したからである。
    </article>
    <!-- 更に gap-x を指定すると、段同士の隙間の大きさを指定できる -->
    <article class="bg-blue-200 indent-4 columns-3 gap-x-24">
      <span class="font-black">（隙間の大きさを指定）</span>
      小使に負ぶさって帰って来た時、おやじが大きな眼をして二階ぐらいから飛び降りて腰を抜か
      す奴があるかと云ったから、この次は抜かさずに飛んで見せますと答えた。（青空文庫より）
    </article>
  </section>
</div>
```

### break-before/inside/afterユーティリティが追加されました（alpha.1）

```html
<!-- columnsとbreak-insideを使用するサンプルコード -->
<div class="space-y-4 p-4">
  <section class="columns-2 space-y-2">
    <!-- ボックスの途中で折り返しをする（デフォルト） -->
    <article class="p-4 border-2 bg-gray-50 border-gray-700">
      親譲りの無鉄砲で小供の時から損ばかりしている。
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700">
      なぜそんな無闇をしたと聞く人があるかも知れぬ。別段深い理由でもない。
      新築の二階から首を出していたら、同級生の一人が冗談に、いくら威張っても、
      そこから飛び降りる事は出来まい。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700">
      弱虫やーい。と囃したからである。小使に負ぶさって帰って来た時、
      おやじが大きな眼をして二階ぐらいから飛び降りて腰を抜かす奴があるかと云ったから、
      この次は抜かさずに飛んで見せますと答えた。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700">
      親譲りの無鉄砲で小供の時から損ばかりしている。
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。
      なぜそんな無闇をしたと聞く人があるかも知れぬ。
      別段深い理由でもない。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700">
      新築の二階から首を出していたら、同級生の一人が冗談に、
      いくら威張っても、そこから飛び降りる事は出来まい。弱虫やーい。
      と囃したからである。小使に負ぶさって帰って来た時、
      おやじが大きな眼をして二階ぐらいから飛び降りて腰を抜かす奴があるかと云ったから、
      この次は抜かさずに飛んで見せますと答えた。
    </article>
  </section>
  <section class="columns-2 space-y-2">
    <!-- ボックスの途中でページ、段、領域の折り返しをしない -->
    <article class="p-4 border-2 bg-gray-50 border-gray-700 break-inside-avoid">
      親譲りの無鉄砲で小供の時から損ばかりしている。
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700 break-inside-avoid">
      なぜそんな無闇をしたと聞く人があるかも知れぬ。別段深い理由でもない。
      新築の二階から首を出していたら、同級生の一人が冗談に、いくら威張っても、
      そこから飛び降りる事は出来まい。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700 break-inside-avoid">
      弱虫やーい。と囃したからである。小使に負ぶさって帰って来た時、
      おやじが大きな眼をして二階ぐらいから飛び降りて腰を抜かす奴があるかと云ったから、
      この次は抜かさずに飛んで見せますと答えた。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700 break-inside-avoid">
      親譲りの無鉄砲で小供の時から損ばかりしている。
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。
      なぜそんな無闇をしたと聞く人があるかも知れぬ。
      別段深い理由でもない。
    </article>
    <article class="p-4 border-2 bg-gray-50 border-gray-700 break-inside-avoid">
      新築の二階から首を出していたら、同級生の一人が冗談に、
      いくら威張っても、そこから飛び降りる事は出来まい。弱虫やーい。
      と囃したからである。小使に負ぶさって帰って来た時、
      おやじが大きな眼をして二階ぐらいから飛び降りて腰を抜かす奴があるかと云ったから、
      この次は抜かさずに飛んで見せますと答えた。
    </article>
  </section>
</div>
```

### border-x / border-y ユーティリティが追加されました（alpha.1）

```html
<!-- border-x / border-y ユーティリティを使用するサンプルコード -->
<div class="space-y-4 p-4">
  <section class="columns-1 space-y-2">
    <!-- 左右だけ罫線を引く -->
    <article class="p-4 border-x-2 bg-gray-50 border-gray-700">
      親譲りの無鉄砲で小供の時から損ばかりしている。
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。
    </article>
    <!-- 上下だけ罫線を引く -->
    <article class="p-4 border-y-2 bg-gray-50 border-gray-700">
      なぜそんな無闇をしたと聞く人があるかも知れぬ。別段深い理由でもない。
      新築の二階から首を出していたら、同級生の一人が冗談に、いくら威張っても、
      そこから飛び降りる事は出来まい。
    </article>
    <!-- 左右と上下で罫線の色を変える -->
    <article class="p-4 border-2 bg-gray-50 border-x-gray-700 border-y-gray-300">
      弱虫やーい。と囃したからである。小使に負ぶさって帰って来た時、
      おやじが大きな眼をして二階ぐらいから飛び降りて腰を抜かす奴があるかと云ったから、
      この次は抜かさずに飛んで見せますと答えた。</article>
  </section>
</div>
```

### file: バリアントが追加されました（alpha.1）

```html
<!-- file: バリアントを使用するサンプルコード -->
<div class="space-y-4 p-4">
  <section class="columns-1 space-y-2">
    <input type="file" class="
    file:bg-red-200 file:text-red-500 file:border-2 file:border-red-500
    file:hover:bg-yellow-200 
    " />
  </section>
</div>
```

### open: バリアントが追加されました（alpha.1）

```html
<!-- open: バリアントを使用して詳細の折りたたみを開閉するサンプルコード -->
<div class="space-y-4 p-4">
  <details class="border-0 open:border-4" open>
      <summary>坊っちゃん</summary>
      親譲りの無鉄砲で小供の時から損ばかりしている。<br/>
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。<br/>
      なぜそんな無闇をしたと聞く人があるかも知れぬ。<br/>
      別段深い理由でもない。
  </details>
  <details class="border-0 open:border-4">
    <summary>坊っちゃん</summary>
      親譲りの無鉄砲で小供の時から損ばかりしている。<br/>
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。<br/>
      なぜそんな無闇をしたと聞く人があるかも知れぬ。<br/>
      別段深い理由でもない。
  </details>
  <details class="group peer" open>
    <summary>
      <span class="hidden group-open:inline">詳細を<span class="font-black">開いて</span>います（子供）</span>
      <span class="inline group-open:hidden">詳細を<span class="font-black">閉じて</span>います（子供）</span>
    </summary>
    group-open、peer-openを使うと、子供や兄弟の要素もopen/closeでスタイルを切り替えできます。
  </details>
  <span class="hidden peer-open:inline">詳細を<span class="font-black">開いて</span>います（兄弟）</span>
  <span class="inline peer-open:hidden">詳細を<span class="font-black">閉じて</span>います（兄弟）</span>
</div>
```

```html
<!-- open: バリアントを使用してモーダルを開閉するサンプルコード -->
<div class="space-y-4 p-4">
  <dialog class="border-0 open:border-4 top-11" open>
    <p>親譲りの無鉄砲で小供の時から損ばかりしている。<br/>
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。<br/>
      なぜそんな無闇をしたと聞く人があるかも知れぬ。<br/>
      別段深い理由でもない。</p>
  </dialog>
  <dialog class="border-0 open:border-4">
      親譲りの無鉄砲で小供の時から損ばかりしている。<br/>
      小学校に居る時分学校の二階から飛び降りて一週間ほど腰を抜かした事がある。<br/>
      なぜそんな無闇をしたと聞く人があるかも知れぬ。<br/>
      別段深い理由でもない。
  </dialog>
  <dialog class="group peer border-0 open:border-4 top-52" open>
    <span class="hidden group-open:inline">モーダルを<span class="font-black">開いて</span>います（子供）</span>
    <span class="inline group-open:hidden">モーダルを<span class="font-black">閉じて</span>います（子供）</span>
    <br/>
    group-open、peer-openを使うと、子供や兄弟の要素もopen/closeでスタイルを切り替えできます。
  </dialog>
  <span class="hidden peer-open:inline">モーダルを<span class="font-black">開いて</span>います（兄弟）</span>
  <span class="inline peer-open:hidden">モーダルを<span class="font-black">閉じて</span>います（兄弟）</span>
</div>
```

### 下線や取り消し線の色が変更できるようになりました（alpha.2）

```html
<!-- 下線と取り消し線の色を変えるサンプルコード -->
<div class="space-y-4 p-4">
  <a href="#" class="block underline decoration-blue-500 hover:decoration-blue-500/50">
    下線の色が青色になります。/のあとに続けて透明度を指定できます。
  </a>
  <span class="block line-through decoration-blue-500 hover:decoration-blue-500/50">
    取り消し線の色が青色になります。/のあとに続けて透明度を指定できます。
  </span>
</div>
```

### print: バリアントが追加されました（alpha.1）

```html
<!-- 下線と取り消し線の色を変えるサンプルコード -->
<nav class="print:hidden">
  印刷するときは、非表示になります
</nav>
<h1 class="text-blue-700 print:text-black">
  印刷するときは、最も太い字になります
</h1>
```

### SVGの塗りつぶしと線の色の記述量が減り、塗りつぶしと線で異なる色を指定できるようになりました（alpha.2）

```html
<!-- v2のときにSVGの塗りつぶしと線の色を変えるサンプルコード -->
<div class="p-4">
  <!-- v2 では、fill-current/stroke-current と text-[色名]-[色の濃さ] を指定する -->
  <svg class="fill-current text-blue-500">
    <circle cx="50" cy="50" r="40" stroke-width="3" />
  </svg>
  <svg class="stroke-current text-blue-500">
    <circle cx="50" cy="50" r="40" stroke-width="3" />
  </svg>
</div>
```

```html
<!-- v3のときにSVGの塗りつぶしと線の色を変えるサンプルコード -->
<div class="p-4">
  <!-- v3 では、塗りつぶしは fill-[色名]-[色の濃さ]、線の色は stroke-[色名]-[色の濃さ] を指定する -->
  <svg class="fill-blue-500">
    <circle cx="50" cy="50" r="40" stroke-width="3" />
  </svg>
  <svg class="stroke-blue-500">
    <circle cx="50" cy="50" r="40" stroke-width="3" />
  </svg>
  <!-- v3 では、線と塗りつぶしの色を同時に別々の色に指定できます -->
  <svg class="fill-blue-500 stroke-red-500">
    <circle cx="50" cy="50" r="40" stroke-width="3" />
  </svg>
</div>
```

### 影に対して色や透明度の指定ができるようになりました（alpha.2）

```html
<!-- v3のときにSVGの塗りつぶしと線の色を変えるサンプルコード -->
<div class="p-4 bg-blue-100 w-screen h-screen flex space-x-5">
  <!-- デフォルトの影の色（v2と同様） -->
  <div class="w-40 h-40 shadow-lg rounded-full bg-white"></div>
  <!-- 影の色を背景色と同系色（blue-400）に変更する -->
  <div class="w-40 h-40 shadow-lg rounded-full bg-white shadow-blue-400"></div>
  <!-- 影の色を背景色と同系色（blue-400）に変更し、透明度（75%）を指定する -->
  <div class="w-40 h-40 shadow-lg rounded-full bg-white shadow-blue-400/75"></div>
  <!-- 影の色を背景色と別系色（yellow-500）に変更し、透明度（80%）を指定し、ネオン調にする -->
  <div class="w-40 h-40 shadow-lg rounded-full bg-white shadow-yellow-500/80"></div>
</div>
```

### 上付き文字、下付き文字を指定するユーティリティが追加されました

```html
<!-- vertical-align で定義できるユーティリティの一覧 -->
<div class="p-4 bg-gray-100 text-2xl underline">
  align<span class="align-top text-xs">top</span><br/>
  align<span class="align-middle text-xs">middle</span><br/>
  align<span class="align-bottom text-xs">bottom</span><br/>
  align<span class="align-super text-xs">super</span><br/>
  align<span class="align-baseline text-xs">baseline</span><br/>
  align<span class="align-sub text-xs">sub</span><br/>
  align<span class="align-text-top text-xs">text-top</span><br/>
  align<span class="align-text-bottom text-xs">text-bottom</span><br/>
  align<span class="align-[0.2rem] text-xs">0.2rem</span><br/>
  align<span class="align-[-1rem] text-xs">-1rem</span><br/>
  align<span class="align-[20%] text-xs">20%</span><br/>
  align<span class="align-[-100%] text-xs">-100%</span>
</div>
```

### フォームのアクセントカラーを指定するユーティリティが追加されました

```html
<!-- フォームのアクセントカラーを指定した例 -->
<div class="p-4 bg-gray-100 space-y-4">
  <input type="checkbox" class="accent-violet-500 w-20 h-20 block" checked />
  <input type="radio" class="accent-violet-500 w-20 h-20 block" checked />
  <input type="range" class="accent-violet-500 w-40 block" />
</div>
```

### 他の競合する境界線も上書きして非表示にするユーティリティ（border-hidden）が追加されました

```html
<!-- 境界線を非表示にするユーティリティの比較例 -->
<div class="p-4">
  <table class="table-auto border-collapse border-8 border-gray-400 w-1/2 text-sm">
    <thead>
      <tr>
        <!-- border-hidden を設定したセルは、競合している周囲のボーダーも含めて消す -->
        <th class="border-hidden border-gray-300">State</th>
        <th class="border-4 border-gray-300">City</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td class="border-4 border-gray-300">Indiana</td>
        <td class="border-4 border-gray-300">Indianapolis</td>
      </tr>
      <tr>
        <!-- border-none を設定したセルは、競合している周囲のボーダーは消さない -->
        <td class="border-none border-gray-300">Ohio</td>
        <td class="border-4 border-gray-300">Columbus</td>
      </tr>
      <tr>
        <td class="border-4 border-gray-300">Michigan</td>
        <td class="border-4 border-gray-300">Detroit</td>
      </tr>
    </tbody>
  </table>
</div>
```

### 親要素の色を継承する属性（inherit）がカラーパレットに追加されました

```html
<!-- 親要素の色を継承する例 -->
<div class="p-4 space-y-4">
  <div class="text-red-500 p-4">
    親要素の文字色は赤色です
    <div class="text-blue-500 hover:text-inherit p-4">
      ホバーしたときの文字色は親要素から継承します
    </div>
  </div>
  <div class="bg-red-500 p-4">
    親要素の背景色は赤色です
    <div class="bg-blue-500 hover:bg-inherit p-4">
      ホバーしたときの背景色は親要素から継承します
    </div>
  </div>
</div>
```

### placeholderバリアントが追加されました

```html
<!-- プレースフォルダーの文字色を変更する例 -->
<div class="p-4 space-y-4">
  <div>
    <!-- テキストボックスのプレースホルダーのスタイルを指定できます -->
    <input type="text" class="placeholder:text-xs placeholder:text-blue-500 border-slate-300 rounded-full border px-3" placeholder="プレースホルダーの文字色を青にする" />
  </div>
  <div>
    <!-- テキストボックスのプレースホルダーのスタイルを指定できます -->
    <textarea type="text" class="placeholder:text-xs placeholder:text-blue-500 placeholder:font-bold border-slate-300 rounded-lg border p-3" placeholder="プレースホルダーの文字色を青にする" ></textarea>
  </div>
</div>
```

### portraitとlandscapeバリアントが追加されました

```html
<!-- portraitとlandscapeを利用した例 -->
<div class="p-4 flex landscape:flex-row portrait:flex-col gap-4">
  <div class="p-4 shadow bg-neutral-600 text-neutral-50">
    縦長（landscape）の画面の場合、縦（row）に並べる
  </div>
  <div class="p-4 shadow bg-neutral-600 text-neutral-50">
    横長（portrait）の画面の場合、横（col）に並べる
  </div>
</div>
```

### 任意の値に対して新機能が追加されました

#### 背景画像（urlの型ヒント）機能が追加されました

```html
<!-- 背景画像のURLを指定した例 -->
<div class="bg-[#0088cc]"></div>
<div class="bg-[url('/url-image.png')]"></div>
<div class="bg-[url:var(--url-css-var)]"></div><!-- CSS変数も使える -->
```

#### 背景画像のサイズ（sizeの型ヒント）機能が追加されました

```html
<!-- 背景画像のサイズを指定した例 -->
<div class="bg-[size:200px_100px]"></div>
```

#### 背景画像の位置（positionの型ヒント）機能が追加されました

```html
<!-- 背景画像の位置を指定した例 -->
<div class="bg-[position:200px_100px]"></div>
```

#### フォントファミリー（familyの型ヒント）機能が追加されました

```html
<!-- familyの型ヒントをCSS変数で指定した例 -->
<div class="font-[family:var(--value)]"></div>
```

#### フォントの太さ（weightの型ヒント）機能が追加されました

```html
<!-- weightの型ヒントをCSS変数で指定した例 -->
<div class="font-[weight:var(--value)]"></div>
```

#### フォントファミリーとフォントの太さの型ヒントが追加された理由

```html
<!-- weightの型ヒントをCSS変数で指定した例 -->
<div class="font-[Georgia]">フォントファミリーを指定</div>
<div class="font-['Gill_Sans']">フォントファミリーを指定</div>
<div class="font-[300]">フォントの太さを指定</div>
<div class="font-[black]">フォントの太さがblackなのか、フォントファミリーのblackのどちらか判別できないため、型ヒントの指定が必要</div>
```

#### CSS関数のmin/max/clampで、length: の型ヒントが不要になりました

```html
<!-- length: の型ヒントを省略した例としない例 -->
<div class="text-[min(10vh,100px)]"></div>
<div class="text-[length:min(10vh,100px)]"></div>
```

#### length: の型ヒントで、アンダーバー区切りで複数の値が設定できるようになりました

```html
<!-- length: の型ヒントで複数の値を指定した例 -->
<div class="bg-[center_top_1rem]"></div>
```

### 任意のプロパティー機能が追加されました

```html
<!-- text-stroke-colorを任意プロパティー機能で指定した例 -->
<div class="p-4 flex flex-row gap-4">
  <div class="[-webkit-text-stroke-color:theme(colors.blue.500)] md:[-webkit-text-stroke-color:theme(colors.red.500)] [-webkit-text-stroke-width:medium] font-black text-3xl">
    Tailwind CSSで未対応のCSSプロパティーが使える。
    <ul class="list-disc list-inside">
      <li>画面サイズが768px以上の場合、文字色が赤色になる。</li>
      <li>768px未満の場合、文字色が青色になる。</li>
    </ul>
  </div>
  <div class="[--my-color:red] sm:[--my-color:blue] [-webkit-text-stroke-color:var(--my-color)] [-webkit-text-stroke-width:thick] font-black text-3xl">
    CSS変数も使える。
  </div>
  <div class="[--my-color:theme(colors.blue.500)] [-webkit-text-stroke-color:var(--my-color)] [-webkit-text-stroke-width:thin] font-black text-3xl">
    CSS変数にTailwind CSSの関数も使える。
  </div>
  <div class="border-2 bg-slate-100">
    <div class="[margin:20px_40px_50px_10px] bg-white">
      アンダーバー区切りの複数の値指定（例えばmargin）にも使える。
    </div>
  </div>
</div>
```
