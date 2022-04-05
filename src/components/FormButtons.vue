<template>
  <div class="buttons" ref="buttons">
    <button
      class="button prev"
      v-if="counter > 0"
      @click="prevHandler"
    >
      Предыдущий шаг
    </button>
    <button
      class="button next"
      v-if="counter < 2"
      @click="nextHandler"
    >
      Следующий шаг
    </button>
    <button
      class="button"
      v-if="counter === 2 && !invalid"
      @click="submitForm"
    >
      Завершить
    </button>
  </div>
</template>

<script>
export default {
  props: {
    counter: {
      type: Number,
      required: true
    },
    invalid: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    prevHandler () {
      this.$emit('changeCounter', this.counter - 1)
    },
    nextHandler () {
      this.$emit('changeCounter', this.counter + 1)
    },
    submitForm () {
      console.log(this.$refs.buttons.previousSibling)
      alert('Клиент успешно создан')
    }
  }
}
</script>

<style scoped lang="scss">
.button {
  position: relative;
  border: 0;
  background: none;
  color: inherit;
  cursor: pointer;
  transition: 0.3s;

  &:hover {
    box-shadow: 0 2px 0 var(--secondary-color);
  }

  &:focus-visible {
    border-radius: 3px;
    outline: var(--outline-color) solid 3px;
  }

  &.prev {
    margin-left: 16px;

    &::before {
      position: absolute;
      left: -16px;
      content: '🠔';
      vertical-align: middle;
    }
  }

  &.next {
    margin-right: 16px;
    margin-left: auto;

    &::after {
      position: absolute;
      right: -16px;
      content: '🠖';
      vertical-align: middle;
    }
  }
}
</style>
