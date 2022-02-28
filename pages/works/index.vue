<template>
  <div class="child">
    <div class="childMainVisual">
      <div class="container container--lg">
        <h1 class="childMainVisual__title">Works</h1>
        <p>成果物をご紹介します。</p>
      </div>
    </div>

    <div class="container">
      <ol class="row works">
        <li class="works__item">
          <nuxt-link to="/works/111" class="works__inner">
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
    </div>
  </div>
</template>

<style lang="scss" scoped>
.childMainVisual {
  text-align: center;
  padding: 5em 0;
  background-color: $base-color-secondary;
  margin-bottom: 2.5em;

  @include mq() {
    margin-bottom: 5em;
  }

  &__title {
    font-family: $font-ubuntu;
    font-size: fz(40);
    margin-bottom: 0.25em;
  }
}

.works {
  list-style: none;

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