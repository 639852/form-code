<template>
  <section class="app">
    <h1 class="title-h1">Форма создания клиента</h1>
    <form class="form">
      <data-wrapper :counter="counter" @changeValid="changeValid" />
    </form>
    <form-buttons
      @changeCounter="changeCounter"
      :counter="counter"
      :invalid="invalid"
    />
  </section>
</template>

<script>
import DataWrapper from './components/DataWrapper.vue'
import FormButtons from './components/FormButtons.vue'

export default {
  name: 'App',
  components: { DataWrapper, FormButtons },
  data () {
    return {
      counter: 0,
      invalid: true
    }
  },
  methods: {
    changeCounter (counter) {
      this.counter = counter
    },
    changeValid (invalid) {
      this.invalid = invalid
    }
  }
}
</script>

<style lang="scss">
@mixin adaptive($property, $pcSize, $mobSize, $maxWidth: 1920) {
  $addSize: $pcSize - $mobSize;
  $maxWidth: $maxWidth - 320;
  #{$property}: calc(#{$mobSize + px} + #{$addSize} * ((100vw - 320px) / #{$maxWidth}));
}

:root {
  --main-color: #2f3138;
  --secondary-color: #fff;
  --background-color: #1d1c26;
  --outline-color: #6d63ca;
}

*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
  background: var(--background-color);
  color: var(--secondary-color);
}

.app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  @include adaptive(gap, 25, 15);
  min-height: 100vh;
  padding: 15px;
}

.form {
  max-width: 450px;
  width: 100%;
  @include adaptive(padding, 25, 15);
  border-radius: 10px;
  overflow: hidden;
  background: var(--main-color);
}

h1.title-h1 {
  padding: 15px 10px;
  text-align: center;
  @extend .form;

  @media (max-width: 768px) {
    @include adaptive(font-size, 24, 21, 768);
  }
}

.buttons {
  display: flex;
  justify-content: space-between;
  @extend .form;
}
</style>
