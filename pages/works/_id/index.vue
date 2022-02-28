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