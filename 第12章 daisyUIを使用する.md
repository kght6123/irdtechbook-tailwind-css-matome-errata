# 12. daisyUIを使用する

## 12.2 daisyUIを使ってみよう

### daisyUIを使えるようにする

```js
// daisyuiの追加
module.exports = {
  content: ["./index.html", "./src/**/*.{vue,js,ts,jsx,tsx}"],
  theme: {
    extend: {
+     colors: require("daisyui/colors"),
    },
  },
- plugins: [],
+ plugins: [require("daisyui")],
};
```

#### コンポーネントを試してみる

```html
<!-- サンプルコードの変更 -->
<!-- src/components/HelloWorld.vue -->
<!-- 省略 -->
- <button type="button" class="bg-yellow-500" @click="count++">
+ <button type="button" class="btn btn-primary" @click="count++">
    count is: {{ count }}
  </button>
<!-- 省略 -->
```

#### Tailwind CSSとの比較をする

```html
<!-- Tailwind CSSでdaisyUIのボタンを実現しようとした場合の例 -->
<!-- src/components/HelloWorld.vue -->
<!-- 省略 -->
- <button type="button" class="bg-yellow-500" @click="count++">
+ <button type="button" class="bg-primary-500 rounded px-2 py-3 text-white font-bold" @click="count++">
    count is: {{ count }}
  </button>
<!-- 省略 -->
```

#### コンポーネントをカスタマイズする

```html
<!-- カスタマイズコードの追加 -->
<!-- src/components/HelloWorld.vue -->
<!-- 省略 -->
- <button type="button" class="btn btn-primary" @click="count++">
+ <button type="button" class="rounded-full btn btn-primary" @click="count++">
    count is: {{ count }}
  </button>
<!-- 省略 -->
```

#### コンポーネントのスタイルを無効化する

```js
// daisyuiのスタイルを無効化
module.exports = {
  content: ["./index.html", "./src/**/*.{vue,js,ts,jsx,tsx}"],
  theme: {
    extend: {
      colors: require("daisyui/colors"),
    },
  },
  plugins: [require("daisyui")],
+ daisyui: {
+   styled: false,
+ },
};
```

#### 全体のテーマを設定する

```html
<!-- 全体をcupcakeテーマに変更 -->
<!-- /index.html -->
<html lang="ja" data-theme="cupcake">
```

#### 一部のテーマを変更する

```html
<!-- テーマ別のサンプルコードに変更 -->
<!-- src/components/HelloWorld.vue -->
<!-- 省略 -->
- <button type="button" class="rounded-full btn btn-primary" @click="count++">
-   count is: {{ count }}
- </button>
+ <div class="flex flex-col p-4 space-y-4 justify-center">
+   <!-- light テーマのボタン -->
+   <button
+     type="button"
+     class="btn btn-primary rounded-full"
+     @click="count++"
+     data-theme="light"
+   >
+     count is: {{ count }}
+   </button>
+   <!-- dark テーマのボタン -->
+   <button
+     type="button"
+     class="btn btn-primary rounded-full"
+     @click="count++"
+     data-theme="dark"
+   >
+     count is: {{ count }}
+   </button>
+   <!-- 全体のテーマ（cupcake）のボタン -->
+   <button type="button" class="btn btn-primary rounded-full" @click="count++">
+     count is: {{ count }}
+   </button>
+ </div>
<!-- 省略 -->
```

#### カスタムテーマを設定する

```js
// カスタムテーマを追加
// tailwind.config.js
  daisyui: {
    themes: [
      {
        /* mythemeというテーマを追加する */
        mytheme: {
          primary: "#a991f7" /* プライマリーカラー */,
          "primary-focus": "#8462f4" /* プライマリーカラー - focused */,
          "primary-content":
            "#ffffff" /* プライマリーカラーの背景色 */,

          secondary: "#f6d860" /* セカンダリカラー color */,
          "secondary-focus": "#f3cc30" /* セカンダリカラー - focused */,
          "secondary-content":
            "#ffffff" /* セカンダリカラーの背景色 */,

          accent: "#37cdbe" /* アクセントカラー */,
          "accent-focus": "#2aa79b" /* アクセントカラー - focused */,
          "accent-content":
            "#ffffff" /* アクセントカラーの背景色 */,

          neutral: "#3d4451" /* ニュートラルカラー */,
          "neutral-focus": "#2a2e37" /* ニュートラルカラー - focused */,
          "neutral-content":
            "#ffffff" /* ニュートラルカラーの背景色 */,

          "base-100":
            "#ffffff" /* ページのベースカラー、ページの背景色に利用される */,
          "base-200": "#f9fafb" /* ベースカラー、少し暗めにする */,
          "base-300": "#d1d5db" /* ベースカラー、より暗めにする */,
          "base-content":
            "#1f2937" /* ベースカラーの背景色 */,

          info: "#2094f3" /* 情報提供のときの色 */,
          success: "#009485" /* 正常のときの色 */,
          warning: "#ff9900" /* 警告のときの色 */,
          error: "#ff5724" /* エラーのときの色 */,
        },
      },
    ],
  },
```

#### CSS変数を設定してカスタマイズする

```css
/* カスタムCSS変数を追加 */
/* src/assets/css/tailwind.css */
@layer components {
  :root {
    --rounded-box: 1rem; /* カードなど大きいコンポーネントの角丸（border-radius）を定義する */
    --rounded-btn: 0.5rem; /* ボタンなど中ぐらいのコンポーネントの角丸（border-radius）を定義する */
    --rounded-badge: 1.9rem; /* バッチなど小さいコンポーネントの角丸（border-radius）を定義する */

    --animation-btn: 0.25s; /* ボタンのアニメーション時間を定義する */
    --animation-input: 0.2s; /* チェックボックスやトグルのアニメーション時間を定義する */

    --padding-card: 2rem; /* カードの余白（padding）を定義する */

    --btn-text-case: lowercase; /* ボタンの text-case を定義する */
    --navbar-padding: 0.5rem; /* ナビゲーションバーの余白（padding）を定義する */
    --border-btn: 1px; /* ボタンのボーダー（border-size）を定義する */
  }
}
```

### daisyUIでブログのトップページを作ろう

#### ヘッダーを作成

```html
<!-- organisms/Header.vueコンポーネントを作成 -->
/* src/components/organisms/Header.vue */
<template>
  <div
    class="navbar mb-2 shadow-lg bg-neutral text-neutral-content rounded-box"
  >
    <div class="px-2 mx-2 navbar-start">
      <span class="text-lg font-bold">まいブログ</span>
    </div>
    <div class="hidden px-2 mx-2 navbar-center lg:flex">
      <div class="flex items-stretch">
        <a class="btn btn-ghost btn-sm rounded-btn">ほーむ</a>
        <a class="btn btn-ghost btn-sm rounded-btn">ぽーとふぃりお</a>
        <a class="btn btn-ghost btn-sm rounded-btn">あばうと</a>
        <a class="btn btn-ghost btn-sm rounded-btn">こんたくと</a>
      </div>
    </div>
    <div class="navbar-end">
      <button class="btn btn-square btn-ghost">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          class="inline-block w-6 h-6 stroke-current"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"
          ></path>
        </svg>
      </button>
      <button class="btn btn-square btn-ghost">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          class="inline-block w-6 h-6 stroke-current"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
          ></path>
        </svg>
      </button>
    </div>
  </div>
</template>
```

#### カルーセルを作成

```html
<!-- organisms/Carousel.vueコンポーネントを作成 -->
/* src/components/organisms/Carousel.vue */
<template>
  <div class="w-full carousel">
    <div id="slide1" class="relative w-full pt-20 carousel-item">
      <img src="../../assets/SAYA160312500I9A3721_TP_V4.jpg" class="w-full" />
      <div class="absolute flex justify-between left-5 right-5 top-1/2">
        <a href="/components/carousel#slide4" class="btn btn-circle">❮</a>
        <a href="/components/carousel#slide2" class="btn btn-circle">❯</a>
      </div>
    </div>
    <div id="slide2" class="relative w-full pt-20 carousel-item">
      <img src="../../assets/SAYA160312170I9A3708_TP_V4.jpg" class="w-full" />
      <div
        class="
          absolute
          flex
          justify-between
          transform
          -translate-y-1/2
          left-5
          right-5
          top-1/2
        "
      >
        <a href="/components/carousel#slide1" class="btn btn-circle">❮</a>
        <a href="/components/carousel#slide3" class="btn btn-circle">❯</a>
      </div>
    </div>
    <div id="slide3" class="relative w-full pt-20 carousel-item">
      <img src="../../assets/SAYAPAKU5460_TP_V4.jpg" class="w-full" />
      <div
        class="
          absolute
          flex
          justify-between
          transform
          -translate-y-1/2
          left-5
          right-5
          top-1/2
        "
      >
        <a href="/components/carousel#slide2" class="btn btn-circle">❮</a>
        <a href="/components/carousel#slide4" class="btn btn-circle">❯</a>
      </div>
    </div>
    <div id="slide4" class="relative w-full pt-20 carousel-item">
      <img src="../../assets/SAYA0I9A8798_TP_V4.jpg" class="w-full" />
      <div
        class="
          absolute
          flex
          justify-between
          transform
          -translate-y-1/2
          left-5
          right-5
          top-1/2
        "
      >
        <a href="/components/carousel#slide3" class="btn btn-circle">❮</a>
        <a href="/components/carousel#slide1" class="btn btn-circle">❯</a>
      </div>
    </div>
  </div>
</template>
```

#### 記事一覧を作成

```html
<!-- molcules/CardItem.vueコンポーネントを作成 -->
/* src/components/molcules/CardItem.vue */
<template>
  <!-- カードの横幅をモバイル端末なら横幅いっぱい、それ以外なら画面の3分の1に指定する -->
  <!-- さらに、カード同士の間の隙間を広げ（p-1）、影（shadow-lg）も追加する -->
  <div class="sm:w-full md:w-1/3 p-1">
    <div class="card bordered shadow-lg">
      <figure>
        <img src="../../assets/SAYAPAKU5027_TP_V4.jpg" class="w-full" />
      </figure>
      <div class="card-body">
        <h2 class="card-title">ブログのタイトル</h2>
        <p>ブログの概要です、素敵なブログにしたいよ。</p>
        <div class="card-actions">
          <div class="badge badge-secondary">記事</div>
          <div class="badge badge-secondary">写真</div>
        </div>
      </div>
    </div>
  </div>
</template>
```

```html
<!-- organisms/BlogItemList.vueコンポーネントを作成 -->
/* src/components/organisms/BlogItemList.vue */
<script setup lang="ts">
<!-- CardItemのコンポーネントをインポートして使う -->
import CardItem from "../molcules/CardItem.vue";
</script>

<template>
  <!-- カードが横幅いっぱいまで並んだら、flex-wrapで折り返しする -->
  <div class="flex w-full px-4 py-4 bg-base-200 flex-wrap">
    <CardItem />
    <CardItem />
    <CardItem />
    <CardItem />
    <CardItem />
    <CardItem />
  </div>
</template>
```

#### ブログの概要を作成

```html
<!-- organisms/Hero.vueコンポーネントを作成 -->
/* src/components/organisms/Hero.vue */
<template>
  <div class="hero bg-base-200 py-8">
    <div class="text-center hero-content">
      <div class="max-w-md space-y-8">
        <h1 class="text-4xl font-bold">DaisyUIで作りました</h1>
        <div>
          <p>
            このぶろぐはTailwind CSSと、そのプラグインのDaisyUIで作られています。
          </p>
          <p>
            DaisyUIには様々なTailwind CSSベースの美しいコンポーネントが用意されており、UIの品質や開発の速度を上げることができます。
          </p>
          <p>また、自由にカスタマイズすることもできます。</p>
        </div>
        <a href="https://daisyui.com/" class="btn btn-primary" target="_blank"
          >Get Started</a
        >
      </div>
    </div>
  </div>
</template>
```

#### フッターを作成

```html
<!-- organisms/Footer.vueコンポーネントを作成 -->
/* src/components/organisms/Footer.vue */
<template>
  <footer class="p-10 footer bg-base-200 text-base-content footer-center">
    <div class="grid grid-flow-col gap-4">
      <a class="link link-hover">あばうと</a>
      <a class="link link-hover">こんたくと</a>
      <a class="link link-hover">じょぶ</a>
      <a class="link link-hover">めんせきじこう</a>
    </div>
    <div>
      <div class="grid grid-flow-col gap-4">
        <a>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            class="fill-current"
          >
            <path
              d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"
            ></path>
          </svg>
        </a>
        <a>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            class="fill-current"
          >
            <path
              d="M19.615 3.184c-3.604-.246-11.631-.245-15.23 0-3.897.266-4.356 2.62-4.385 8.816.029 6.185.484 8.549 4.385 8.816 3.6.245 11.626.246 15.23 0 3.897-.266 4.356-2.62 4.385-8.816-.029-6.185-.484-8.549-4.385-8.816zm-10.615 12.816v-8l8 3.993-8 4.007z"
            ></path>
          </svg>
        </a>
        <a>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            class="fill-current"
          >
            <path
              d="M9 8h-3v4h3v12h5v-12h3.642l.358-4h-4v-1.667c0-.955.192-1.333 1.115-1.333h2.885v-5h-3.808c-3.596 0-5.192 1.583-5.192 4.615v3.385z"
            ></path>
          </svg>
        </a>
      </div>
    </div>
    <div>
      <p>Copyright © 2021 - All right reserved by kght6123.</p>
    </div>
  </footer>
</template>
```

#### 作成したコンポーネントを組み合わせます

```html
<!-- App.vueコンポーネントを修正 -->
/* src/App.vue */
<script setup lang="ts">
// 作成した生体コンポーネントを一通り、インポートする
import Header from "./components/organisms/Header.vue";
import Carousel from "./components/organisms/Carousel.vue";
import BlogItemList from "./components/organisms/BlogItemList.vue";
import Hero from "./components/organisms/Hero.vue";
import Footer from "./components/organisms/Footer.vue";
</script>

<template>
  <!-- ヘッダの横幅を調整し、上部に固定 -->
  <Header class="fixed w-screen z-10" />
  <Carousel />
  <BlogItemList />
  <!-- 区切り線を入れる -->
  <div class="divider"></div>
  <Hero />
  <!-- 区切り線を入れる -->
  <div class="divider"></div>
  <Footer />
</template>
```

#### 完成イメージ

```html
<!-- 全体をlightテーマに変更 -->
<!-- /index.html -->
<html lang="ja" data-theme="light">
```

```html
<!-- 全体をdarkテーマに変更 -->
<!-- /index.html -->
<html lang="ja" data-theme="dark">
```
