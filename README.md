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