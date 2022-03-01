# microcms_nuxt_jamstack_portfolio_sample

## 0. インデックス
1. プロジェクトの作成
1. Sassの設定
    1. Sassのインストール
    1. Sass変数と関数を作成する(variables/functions/mixins)
    1. Sass変数や関数をグローバル化する
    1. ベーススタイルの設定
    1. グローバルスタイルの設定
1. Google Fontsの読み込み
1. ヘッダーとフッターの作成
1. 各ページのコーディング
    1. TOPページ
    1. Works一覧ページ
    1. Works詳細ページ
1. 環境変数を作成
1. nuxt-microcms-moduleのインストールと設定
1. APIを取得
    1. TOPページ
    1. Works一覧ページ
    1. Works詳細ページ
1. date-fnsモジュールを使用し日付をフォーマット

## 1. プロジェクトの作成

```bash
$ yarn create nuxt-app portfolio
```

## 2. Sassの設定

### 2-1. Sassのインストール

```bash
$ yarn add -D sass sass-loader@10
```

### 2-2. Sass変数と関数を作成する

#### assets/scss/settings/_variables.scss

```scss
// -------------------------------------------
// Color Settings
// -------------------------------------------
// base
$base-color-primary: #fff;
$base-color-secondary: #fafafa;

// text color
$text-color-primary: #010101;
$text-color-secondary: #fff;

// key
$key-color-black: #010101;

// background
$color-body-background: $base-color-primary;

// -------------------------------------------
// Typeface Settings
// -------------------------------------------
$font-base: 'ヒラギノ角ゴ Pro W3', 'Hiragino Kaku Gothic Pro', 'Osaka',
  'メイリオ', 'Meiryo', 'ＭＳ Ｐゴシック', 'MS P Gothic', sans-serif;

// -------------------------------------------
// Break Points
// -------------------------------------------
$breakpoints: (
  'sm': 480px,
  'md': 768px,
  'lg': 1024px,
  'xl': 1200px,
  'xxl': 1440px,
) !default;
```

#### assets/scss/settings/_functions.scss

```scss
@use 'sass:math';

/**
 * 引数のfontSizeをremに変換する関数
 * @param fontSize フォントサイズ
 */
@function fz($fontSize) {
  @return math.div($fontSize, 16) * 1rem;
}
```

#### assets/scss/settings/_mixins.scss

```scss
@use 'sass:map';
@use 'variables' as var;
//============================================
//  MIXIN
//============================================
// Media Queries
@mixin mq($breakpoint: md, $type: min) {
  @if map.has-key(var.$breakpoints, $breakpoint) {
    @if ($type == max) {
      $width: map.get(var.$breakpoints, $breakpoint);
      $width: $width - 1;
      @media screen and (max-width: #{$width}) {
        @content;
      }
    } @else if($type == min) {
      @media screen and (min-width: #{map.get(var.$breakpoints, $breakpoint)}) {
        @content;
      }
    }
  }
}
```

#### assets/scss/app.scss

```scss
@import './settings/variables';
@import './settings/functions';
@import './settings/mixins';
```

### 2-3. Sass変数や関数をグローバル化する

```bash
yarn add -D @nuxtjs/style-resources
```

#### nuxt.config.js

```javascript
export default {
  //...

  buildModules: [
    '@nuxtjs/style-resources', // 追加
  ],

  // ...

  styleResources: {
    scss: ['~/assets/scss/app.scss'],
    hoistUseStatements: true,
  },
}
```

### 2-4. ベーススタイルの設定
[ress](https://github.com/filipelinhares/ress)

```bash
$ yarn add ress
```

#### nuxt.config.js

```javascript
export default {
  // ...

  css: ['ress'],

  // ...
}
```

#### assets/scss/base.scss

```scss
html {
  box-sizing: border-box;
  height: 100%;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}

body {
  height: 100%;
  font-family: $font-base;
  font-size: fz(16);
  line-height: 1.5;
  color: $text-color-primary;
  letter-spacing: 0.05em;
  background-color: $color-body-background;
}

img {
  max-width: 100%;
  height: auto;
  vertical-align: bottom;
}

a {
  color: inherit;
  text-decoration: none;
}

h1,
h2,
h3,
h4,
h5,
strong {
  font-weight: bold;
}

input,
textarea {
  max-width: 100%;
  font-family: inherit;
  font-size: 100%;
}
```

#### nuxt.config.js

```javascript
export default {
  // ...

  css: ['ress', '~/assets/scss/base.scss'],

  // ...
}
```

### 2-5. グローバルスタイルの設定

#### assets/scss/global.scss

```scss
.visuallyHidden {
  position: absolute;
  top: 0;
  left: 0;
  white-space: nowrap;
  width: 1px;
  height: 1px;
  overflow: hidden;
  border: 0;
  padding: 0;
  clip: rect(0 0 0 0);
  clip-path: inset(50%);
  margin: -1px;
}

.container {
  max-width: 780px;
  width: 90%;
  margin-left: auto;
  margin-right: auto;

  &--lg {
    max-width: 960px;
  }
}

.row {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.child {
  padding-bottom: 5em;
}

.background--gray {
  background-color: $base-color-secondary;
}

.sectionPrimary {
  padding: 5em 0;

  @include mq() {
    padding: 7.5em 0;
  }
}

.headingPrimary {
  font-family: $font-ubuntu;
  font-size: fz(40);
  font-weight: bold;
  text-transform: capitalize;
  text-align: center;
  margin-bottom: 1em;
}

.button-area {
  text-align: center;
  margin-top: 2em;

  @include mq() {
    margin-top: 2.5em;
  }
}

.buttonPrimary {
  color: $key-color-black;
  display: inline-block;
  font-family: $font-ubuntu;
  font-size: fz(18);
  font-weight: bold;
  text-transform: capitalize;
  text-align: center;
  text-indent: -1em;
  line-height: 56px;
  padding: 0 1em;
  border: 2px solid;
  border-radius: 4px;
  min-width: 230px;
  position: relative;

  &::after {
    content: '';
    display: inline-block;
    width: 24px;
    height: 24px;
    background: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTkuNzUgNy41MDI0NEwxNSAxMi4wMDAyTDkuNzUgMTYuNTAyNCIgc3Ryb2tlPSIjMDEwMTAxIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIi8+Cjwvc3ZnPgo=)
      center no-repeat;
    background-size: contain;
    position: absolute;
    top: 50%;
    right: 1em;
    transform: translateY(-50%);
    transition: all 0.3s ease-in-out;
  }

  &:hover {
    &::after {
      transform: translate(5px, -50%);
    }
  }

  &--leftArrow {
    text-indent: 1em;

    &::after {
      background-image: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTE0LjI1IDcuNTAyNDRMOSAxMi4wMDAyTDE0LjI1IDE2LjUwMjQiIHN0cm9rZT0iIzAxMDEwMSIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCIvPgo8L3N2Zz4K);
      background-size: contain;
      right: inherit;
      left: 1em;
    }

    &:hover {
      &::after {
        transform: translate(-5px, -50%);
      }
    }
  }
}
```

```javascript
export default {
  // ...

  css: ['ress', '~/assets/scss/base.scss', '~/assets/scss/global.scss'],

  // ...
}
```

## 3. Google Fontsの読み込み

```bash
$ yarn add nuxt-webfontloader
```

#### nuxt.config.js

```javascript
export default {
  // ...

  modules: [
    'nuxt-webfontloader',
  ],

  //...

  webfontloader: {
    google: {
      families: ['Ubuntu:wght@400,700&display=swap'],
    },
  },

  // ...
}
```

#### assets/scss/settings/_variables.scss

```scss
$font-ubuntu: 'Ubuntu', sans-serif;
```

## 4. ヘッダーとフッターの作成

#### components/TheHeader.vue

```html
<template>
  <header class="header">
    <div class="headerContainer">
      <!-- ロゴ -->
      <component :is="isTopPage ? 'h1' : 'p'" class="logo">
        <nuxt-link to="/">My Portfolio</nuxt-link>
      </component>

      <!-- メニュー -->
      <nav>
        <ul class="menu">
          <li>
            <nuxt-link to="/#about" class="menu__link">about</nuxt-link>
          </li>
          <li>
            <nuxt-link to="/works" class="menu__link">works</nuxt-link>
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  computed: {
    // TOPページかどうか
    isTopPage() {
      if (this.$route.name === 'index') return true
      return false
    },
  },
}
</script>

<style lang="scss" scoped>
.header {
  width: 100%;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  background-color: $base-color-primary;
}

.headerContainer {
  padding: 0 1.5em;
  display: flex;
  justify-content: space-between;
  align-items: center;

  @include mq() {
    padding: 0 4em;
  }
}

.logo {
  color: $text-color-primary;
  font-family: $font-ubuntu;
  font-size: fz(18);
  font-weight: bold;
  letter-spacing: 0;

  @include mq() {
    font-size: fz(24);
  }
}

.menu {
  list-style: none;
  display: flex;
  margin-right: -0.75em;

  &__link {
    font-family: $font-ubuntu;
    font-weight: bold;
    text-transform: capitalize;
    line-height: 64px;
    display: inline-block;
    padding: 0 0.25em;
    position: relative;

    @include mq() {
      padding: 0 0.75em;
    }

    &::after {
      content: '';
      display: block;
      width: 0;
      height: 2px;
      background-color: $key-color-black;
      transition: all 0.3s ease-in-out;
      position: absolute;
      left: 0;
      bottom: 0;
    }

    &:hover {
      &::after {
        width: 100%;
      }
    }
  }
}
</style>
```

#### components/TheFooter.vue

```html
<template>
  <footer class="footer">
    <p class="copyright">
      <small>©️2021 My Portfolio All Rights Reserved.</small>
    </p>
  </footer>
</template>

<style lang="scss" scoped>
.footer {
  padding: 1em 0;
  background-color: $base-color-secondary;
}

.copyright {
  font-size: fz(14);
  font-family: $font-ubuntu;
  text-align: center;
}
</style>
```

#### layouts/default.vue
ヘッダーとフッターはサイト全体で共通化する

```html
<template>
  <div class="wrapper">
    <TheHeader class="header" />
    <main class="main">
      <Nuxt />
    </main>
    <TheFooter />
  </div>
</template>

<style lang="scss" scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.header {
  position: fixed;
  z-index: 100;
}

.main {
  flex: 1;
  overflow-x: hidden;
  padding-top: 4em;
}
</style>
```

## 5. 各ページのコーディング
microCMSのAPIを流し込む想定で各ページをコーディングする

### 5-1. TOPページ

#### pages/index.vue

```html
<template>
  <div>
    <div class="mainVisual">
      <picture>
        <source
          srcset="https://placehold.jp/375x530.png"
          media="(max-width: 767px)"
        />
        <img src="https://placehold.jp/1440x436.png" alt="" />
      </picture>
    </div>

    <section id="about" class="sectionPrimary">
      <div class="container">
        <h2 class="headingPrimary">about</h2>
        <div class="profile">
          <div class="profile__upper">
            <div class="profile__text">
              <p class="profile__name">
                山田 太郎<span lang="en">Taro Yamada</span>
              </p>
              <dl class="profile__item">
                <dt class="profile__title">技術スタック</dt>
                <dd>HTML / CSS / jQuery / JavaScript / Nuxt</dd>
              </dl>
              <dl class="profile__item">
                <dt class="profile__title">趣味</dt>
                <dd>開発、ゲーム、YouTube、ライブ、音楽フェス、ピアノ、卓球</dd>
              </dl>
            </div>
            <figure class="profile__image">
              <img src="https://placehold.jp/260x260.png" alt="your name" />
            </figure>
          </div>
          <p class="profile__message">
            自己紹介を入れましょう。出身や経歴と現在の仕事の内容を簡単に話すも良し。<br />数年後の目標や今学んでいること、活動している内容を入れるのも良いかと思います。
          </p>
        </div>
      </div>
    </section>

    <section class="sectionPrimary background--gray">
      <div class="container">
        <h2 class="headingPrimary">works</h2>
        <ol class="row works">
          <li class="works__item">
            <nuxt-link to="#!" class="works__inner">
              <figure class="works__image">
                <img src="https://placehold.jp/370x229.png" alt="" />
              </figure>
              <div class="works__text">
                <p class="works__name">作品名</p>
                <p class="works__date">
                  <time datetime="2021-12-16">2021.12.16</time>
                </p>
              </div>
            </nuxt-link>
          </li>
          <li class="works__item">
            <nuxt-link to="#!" class="works__inner">
              <figure class="works__image">
                <img src="https://placehold.jp/370x229.png" alt="" />
              </figure>
              <div class="works__text">
                <p class="works__name">作品名</p>
                <p class="works__date">
                  <time datetime="2021-12-16">2021.12.16</time>
                </p>
              </div>
            </nuxt-link>
          </li>
        </ol>
        <p class="button-area">
          <nuxt-link to="/works" class="buttonPrimary">view more</nuxt-link>
        </p>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>
.mainVisual {
  img {
    width: 100%;
  }
}

.profile {
  &__upper {
    display: flex;
    flex-direction: column-reverse;
    margin-bottom: 0.5em;

    @include mq() {
      flex-direction: row-reverse;
      justify-content: space-between;
      margin-bottom: 2em;
    }
  }

  &__text {
    @include mq() {
    }
  }

  &__name {
    font-size: fz(24);
    font-weight: bold;
    margin-bottom: 0.5em;

    @include mq() {
      font-size: fz(28);
      margin-bottom: 0.857em;
    }

    [lang='en'] {
      font-size: fz(18);

      &::before {
        content: '/';
        margin: 0 0.5em;
      }
    }
  }

  &__item {
    margin-bottom: 0.5em;

    @include mq() {
      margin-bottom: 1em;
    }
  }

  &__title {
    font-size: fz(18);
    font-weight: bold;
    margin-bottom: 0.222em;

    @include mq() {
      margin-bottom: 0.444em;
    }
  }

  &__image {
    width: 100%;
    margin-bottom: 1.75em;

    @include mq() {
      width: 40%;
      margin: 0 2em 0 0;
    }

    img {
      width: 100%;
    }
  }

  &__message {
    white-space: pre-wrap;
  }
}

.works {
  list-style: none;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;

  &__item {
    width: 100%;

    @include mq() {
      width: calc((100% - 2.5em) / 2);
    }
  }

  &__item + &__item {
    margin-top: 1.5em;

    @include mq() {
      margin: 0;
    }
  }

  &__inner {
    display: block;
  }

  &__image {
    margin-bottom: 0.5em;

    img {
      width: 100%;
    }
  }

  &__name {
    font-weight: bold;
  }

  &__date {
    font-size: fz(14);
  }
}
</style>
```

### 5-2. Works一覧ページ

#### pages/works/index.vue

```html
<template>
  <div>
    <div class="mainVisual">
      <picture>
        <source
          srcset="https://placehold.jp/375x530.png"
          media="(max-width: 767px)"
        />
        <img src="https://placehold.jp/1440x436.png" alt="" />
      </picture>
    </div>

    <section id="about" class="sectionPrimary">
      <div class="container">
        <h2 class="headingPrimary">about</h2>
        <div class="profile">
          <div class="profile__upper">
            <div class="profile__text">
              <p class="profile__name">
                山田 太郎<span lang="en">Taro Yamada</span>
              </p>
              <dl class="profile__item">
                <dt class="profile__title">技術スタック</dt>
                <dd>HTML / CSS / jQuery / JavaScript / Nuxt</dd>
              </dl>
              <dl class="profile__item">
                <dt class="profile__title">趣味</dt>
                <dd>開発、ゲーム、YouTube、ライブ、音楽フェス、ピアノ、卓球</dd>
              </dl>
            </div>
            <figure class="profile__image">
              <img src="https://placehold.jp/260x260.png" alt="your name" />
            </figure>
          </div>
          <p class="profile__message">
            自己紹介を入れましょう。出身や経歴と現在の仕事の内容を簡単に話すも良し。<br />数年後の目標や今学んでいること、活動している内容を入れるのも良いかと思います。
          </p>
        </div>
      </div>
    </section>

    <section class="sectionPrimary background--gray">
      <div class="container">
        <h2 class="headingPrimary">works</h2>
        <ol class="row works">
          <li class="works__item">
            <nuxt-link to="#!" class="works__inner">
              <figure class="works__image">
                <img src="https://placehold.jp/370x229.png" alt="" />
              </figure>
              <div class="works__text">
                <p class="works__name">作品名</p>
                <p class="works__date">
                  <time datetime="2021-12-16">2021.12.16</time>
                </p>
              </div>
            </nuxt-link>
          </li>
          <li class="works__item">
            <nuxt-link to="#!" class="works__inner">
              <figure class="works__image">
                <img src="https://placehold.jp/370x229.png" alt="" />
              </figure>
              <div class="works__text">
                <p class="works__name">作品名</p>
                <p class="works__date">
                  <time datetime="2021-12-16">2021.12.16</time>
                </p>
              </div>
            </nuxt-link>
          </li>
        </ol>
        <p class="button-area">
          <nuxt-link to="/works" class="buttonPrimary">view more</nuxt-link>
        </p>
      </div>
    </section>
  </div>
</template>

<style lang="scss" scoped>
.mainVisual {
  img {
    width: 100%;
  }
}

.profile {
  &__upper {
    display: flex;
    flex-direction: column-reverse;
    margin-bottom: 0.5em;

    @include mq() {
      flex-direction: row-reverse;
      justify-content: space-between;
      margin-bottom: 2em;
    }
  }

  &__text {
    @include mq() {
    }
  }

  &__name {
    font-size: fz(24);
    font-weight: bold;
    margin-bottom: 0.5em;

    @include mq() {
      font-size: fz(28);
      margin-bottom: 0.857em;
    }

    [lang='en'] {
      font-size: fz(18);

      &::before {
        content: '/';
        margin: 0 0.5em;
      }
    }
  }

  &__item {
    margin-bottom: 0.5em;

    @include mq() {
      margin-bottom: 1em;
    }
  }

  &__title {
    font-size: fz(18);
    font-weight: bold;
    margin-bottom: 0.222em;

    @include mq() {
      margin-bottom: 0.444em;
    }
  }

  &__image {
    width: 100%;
    margin-bottom: 1.75em;

    @include mq() {
      width: 40%;
      margin: 0 2em 0 0;
    }

    img {
      width: 100%;
    }
  }

  &__message {
    white-space: pre-wrap;
  }
}

.works {
  list-style: none;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;

  &__item {
    width: 100%;

    @include mq() {
      width: calc((100% - 2.5em) / 2);
    }
  }

  &__item + &__item {
    margin-top: 1.5em;

    @include mq() {
      margin: 0;
    }
  }

  &__inner {
    display: block;
  }

  &__image {
    margin-bottom: 0.5em;

    img {
      width: 100%;
    }
  }

  &__name {
    font-weight: bold;
  }

  &__date {
    font-size: fz(14);
  }
}
</style>
```

### 5-3. Works詳細ページ

#### pages/works/_id/index.vue

```html
<template>
  <div class="child">
    <div class="worksMainVisual">
      <div class="container container--lg worksMainVisual__inner">
        <div class="worksMainVisual__contents">
          <h1 class="worksMainVisual__title">作品名</h1>
          <dl class="worksMainVisual__item">
            <dt class="worksMainVisual__itemName">リリース日</dt>
            <dd><time datetime="2021-12-16">2021.12.16</time></dd>
          </dl>
          <dl class="worksMainVisual__item">
            <dt class="worksMainVisual__itemName">制作期間</dt>
            <dd>3ヶ月</dd>
          </dl>
          <p>
            複素数体であれば、任意のCM-タイプの A
            は、実際、数体である定義体（英語版）(field of
            definition)を持っている。自己準同型環の可能なタイプは、対合（ロサチの対合（英語版）(Rosati
            involution）をもつ環として既に分類されていて、
          </p>
        </div>
        <figure class="worksMainVisual__thumbnail">
          <img src="https://placehold.jp/540x334.png" alt="" />
        </figure>
      </div>
    </div>

    <div class="container">
      <dl class="worksItem">
        <dt class="worksItem__title">URL</dt>
        <dd class="worksItem__contents">
          <a href="" target="_blank">https://test.com/</a>
        </dd>
      </dl>
      <dl class="worksItem">
        <dt class="worksItem__title">ポジション</dt>
        <dd class="worksItem__contents">テキスト</dd>
      </dl>
      <dl class="worksItem">
        <dt class="worksItem__title">担当</dt>
        <dd class="worksItem__contents">テキスト</dd>
      </dl>
      <dl class="worksItem">
        <dt class="worksItem__title">技術</dt>
        <dd class="worksItem__contents">テキスト</dd>
      </dl>
      <dl class="worksItem">
        <dt class="worksItem__title">ツール</dt>
        <dd class="worksItem__contents">テキスト</dd>
      </dl>
      <dl class="worksItem">
        <dt class="worksItem__title">アピールポイント</dt>
        <dd class="worksItem__contents">
          吾輩は猫である。名前はまだ無い。どこで生れたかとんと見当がつかぬ。何でも薄暗いじめじめした所でニャーニャー泣いていた事だけは記憶している。吾輩はここで始めて人間というものを見た。
          しかもあとで聞くとそれは書生という人間中で一番獰悪な種族であったそうだ。この書生というのは時々我々を捕えて煮て食うという話である。しかしその当時は何という考もなかったから別段恐しいとも思わなかった。ただ彼の掌に載せられてスーと持ち上げられた時何だかフワフワした感じがあったばかりである。
          掌の上で少し落ちついて書生の顔を見たのがいわゆる人間というものの見始であろう。この時妙なものだと思った感じが今でも残っている。第一毛をもって装飾されべきはずの顔がつるつるしてまるで薬缶だ。その後猫にもだいぶ逢ったがこんな片輪には一度も出会わした事がない。のみならず顔の真中があまりに突起している。そうしてその穴の中から時々ぷうぷうと煙を
        </dd>
      </dl>
    </div>

    <p class="button-area">
      <nuxt-link to="/works" class="buttonPrimary buttonPrimary--leftArrow"
        >back</nuxt-link
      >
    </p>
  </div>
</template>

<style lang="scss" scoped>
.worksMainVisual {
  padding: 5em 0;
  background-color: $base-color-secondary;
  margin-bottom: 2.5em;

  @include mq() {
    margin-bottom: 5em;
  }

  &__inner {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    flex-direction: column-reverse;

    @include mq() {
      flex-direction: row;
    }
  }

  &__contents {
    @include mq() {
      width: 39.58%;
    }
  }

  &__thumbnail {
    width: 100%;
    margin-bottom: 1.75em;

    @include mq() {
      width: 56.25%;
    }

    img {
      width: 100%;
    }
  }

  &__title {
    font-size: fz(32);
    margin-bottom: 0.125em;
  }

  &__item {
    margin-bottom: 0.5em;
  }

  &__itemName {
    font-size: fz(18);
    font-weight: bold;
    margin-bottom: 0.222em;
  }
}

.worksItem {
  &__title {
    font-size: fz(24);
    font-weight: bold;
    margin-bottom: 0.333em;
  }

  &__contents {
    white-space: pre-wrap;

    span + span {
      &::before {
        content: '/';
        display: inline-block;
        margin: 0 0.5em;
      }
    }
  }

  + .worksItem {
    margin-top: 1.5em;
  }
}
</style>
```

## 6. 環境変数を作成

#### .env

```
GET_API_KEY=your_api_key
SERVICE_DOMAIN=your_service_domain
```

## 7. nuxt-microcms-moduleのインストールと設定

```bash
$ yarn add nuxt-microcms-module
```

#### nuxt.config.js

```javascript
export default {

  // ...

  modules: [
    'nuxt-webfontloader', // 追加
    'nuxt-microcms-module',
  ],

  // ...
  
  styleResources: {
    scss: ['~/assets/scss/app.scss'],
    hoistUseStatements: true,
  },
  webfontloader: {
    google: {
      families: ['Ubuntu:wght@400,700&display=swap'],
    },
  },
  // 追加
  microcms: {
    options: {
      serviceDomain: process.env.SERVICE_DOMAIN,
      apiKey: process.env.GET_API_KEY,
    },
    mode: process.env.NODE_ENV === 'production' ? 'server' : 'all',
  },
}
```

## 8. APIを取得

#### pages/index.vue

```html
// ...

<div class="container">
  <!-- 差し替えここから -->
  <ol class="row works">
    <li v-for="work in works.contents" :key="work.id" class="works__item">
      <nuxt-link :to="`/works/${work.id}/`" class="works__inner">
        <figure class="works__image">
          <img
            :width="work.thumbnail.width"
            :height="work.thumbnail.height"
            :src="work.thumbnail.url"
            :alt="work.title"
          />
        </figure>
        <div class="works__text">
          <p class="works__name">{{ work.title }}</p>
          <p class="works__date">
            <time :datetime="work.release">{{ work.release }}</time>
          </p>
        </div>
      </nuxt-link>
    </li>
  </ol>
  <!-- 差し替えここまで -->
</div>

// ...

<script>
export default {
  async asyncData({ $microcms }) {
    const works = await $microcms.get({
      endpoint: 'works',
      queries: { limit: 2 },
    })
    return {
      works,
    }
  },
}
</script>

// ...
```

#### pages/works/index.vue

```html
// ...

<section class="sectionPrimary background--gray">
  <div class="container">
    <h2 class="headingPrimary">works</h2>
    <!-- 差し替えここから -->
    <ol class="row works">
      <li v-for="work in works.contents" :key="work.id" class="works__item">
        <nuxt-link :to="`/works/${work.id}/`" class="works__inner">
          <figure class="works__image">
            <img
              :width="work.thumbnail.width"
              :height="work.thumbnail.height"
              :src="work.thumbnail.url"
              :alt="work.title"
            />
          </figure>
          <div class="works__text">
            <p class="works__name">{{ work.title }}</p>
            <p class="works__date">
              <time :datetime="work.release">{{ work.release }}</time>
            </p>
          </div>
        </nuxt-link>
      </li>
    </ol>
    <!-- 差し替えここまで -->
    <p class="button-area">
      <nuxt-link to="/works" class="buttonPrimary">view more</nuxt-link>
    </p>
  </div>
</section>

// ...

<script>
export default {
  async asyncData({ $microcms }) {
    const works = await $microcms.get({
      endpoint: 'works',
    })
    return {
      works,
    }
  },
}
</script>

// ...
```

#### pages/works/_id/index.vue
必須項目ではないものにはv-ifを使用してレンダリングさせないようにする

```html
<template>
  <div class="child">
    <div class="worksMainVisual">
      <div class="container container--lg worksMainVisual__inner">
        <div class="worksMainVisual__contents">
          <h1 class="worksMainVisual__title">{{ work.title }}</h1>
          <dl v-if="work.release" class="worksMainVisual__item">
            <dt class="worksMainVisual__itemName">リリース日</dt>
            <dd>
              <time :datetime="work.release">{{ work.release }}</time>
            </dd>
          </dl>
          <dl v-if="work.term" class="worksMainVisual__item">
            <dt class="worksMainVisual__itemName">制作期間</dt>
            <dd>{{ work.term }}</dd>
          </dl>
          <p v-if="work.overview">{{ work.overview }}</p>
        </div>
        <figure class="worksMainVisual__thumbnail">
          <img
            :width="work.thumbnail.width"
            :height="work.thumbnail.height"
            :src="work.thumbnail.url"
            :alt="work.title"
          />
        </figure>
      </div>
    </div>

    <div class="container">
      <dl class="worksItem">
        <dt class="worksItem__title">URL</dt>
        <dd class="worksItem__contents">
          <a :href="work.url" target="_blank">{{ work.url }}</a>
        </dd>
      </dl>
      <dl v-if="work.position" class="worksItem">
        <dt class="worksItem__title">ポジション</dt>
        <dd class="worksItem__contents">{{ work.position }}</dd>
      </dl>
      <dl v-if="work.responsibility" class="worksItem">
        <dt class="worksItem__title">担当</dt>
        <dd class="worksItem__contents">
          <span
            v-for="(res, resIndex) in work.responsibility"
            :key="resIndex"
            v-text="res"
          />
        </dd>
      </dl>
      <dl class="worksItem">
        <dt class="worksItem__title">技術</dt>
        <dd class="worksItem__contents">
          <span
            v-for="(skill, skillIndex) in work.skill"
            :key="skillIndex"
            v-text="skill"
          />
        </dd>
      </dl>
      <dl v-if="work.tools" class="worksItem">
        <dt class="worksItem__title">ツール</dt>
        <dd class="worksItem__contents">
          <span
            v-for="(tool, toolIndex) in work.tools"
            :key="toolIndex"
            v-text="tool"
          />
        </dd>
      </dl>
      <dl class="worksItem">
        <dt class="worksItem__title">アピールポイント</dt>
        <dd class="worksItem__contents">{{ work.points }}</dd>
      </dl>
    </div>

    <p class="button-area">
      <nuxt-link to="/works" class="buttonPrimary buttonPrimary--leftArrow"
        >back</nuxt-link
      >
    </p>
  </div>
</template>

// ...

<script>
export default {
  async asyncData({ $microcms, params }) {
    const work = await $microcms.get({
      endpoint: `works/${params.id}`,
    })
    return {
      work,
    }
  },
}
</script>

// ...
```

## 9. date-fnsモジュールを使用し日付をフォーマット

```bash
$ yarn add --dev @nuxtjs/date-fns
```

#### nuxt.config.js

```javascript
export default {

  // ...

  // Modules: https://go.nuxtjs.dev/config-modules
  modules: [
    'nuxt-webfontloader', // 追加
    'nuxt-microcms-module', // 追加
  ],

  // ...

}
```

#### pages/works/_id/index.vue

```html
// ...

<dl v-if="work.release" class="worksMainVisual__item">
  <dt class="worksMainVisual__itemName">リリース日</dt>
  <dd>
    <!-- 差し替えここから -->
    <time
      :datetime="work.release"
      v-text="$dateFns.format(new Date(work.release), 'yyyy.MM.dd')"
    />
    <!-- 差し替えここまで -->
  </dd>
</dl>

// ...
```