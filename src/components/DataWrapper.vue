<template>
  <div class="data-wrapper">
    <personal-data @changeValid="changeValid" />
    <address-data @changeValid="changeValid" />
    <passport-data @changeValid="changeValid" />
  </div>
</template>

<script>
import AddressData from './AddressData'
import PassportData from './PassportData.vue'
import PersonalData from './PersonalData'

export default {
  components: {
    PersonalData,
    AddressData,
    PassportData
  },
  data () {
    return {
      sliderWidth: 0,
      sliderMargin: 0,
      personal: true,
      address: true,
      passport: true
    }
  },
  props: {
    counter: {
      type: Number,
      required: true
    }
  },
  computed: {
    allInvalid () {
      return this.personal || this.address || this.passport
    }
  },
  mounted () {
    this.initSlider()

    window.addEventListener('resize', this.initSlider)
  },
  methods: {
    initSlider () {
      const slider = document.querySelector('.form')
      const sliderPaddingBottom = getComputedStyle(slider).paddingBottom
      const sliderWrapperGap = getComputedStyle(this.$el).gap

      this.sliderWidth = slider.offsetWidth - parseFloat(sliderPaddingBottom) * 2
      this.$el.childrenMargin = parseFloat(sliderWrapperGap) * (this.$el.children.length - 1)

      Array.from(this.$el.children).forEach(el => {
        el.style.width = this.sliderWidth + 'px'
      })

      this.$el.style.width = this.sliderWidth * this.$el.children.length + this.$el.childrenMargin + 'px'

      this.moveSlider()
    },
    moveSlider () {
      const sumOfMargin = this.$el.childrenMargin / (this.$el.children.length - 1) * this.counter

      this.$el.style.height = this.$el.children[this.counter].offsetHeight + 'px'

      if (this.counter === 0) {
        this.$el.style.transform = 'translateX(0px)'
      } else {
        this.$el.style.transform = `translateX(-${this.counter * this.sliderWidth + sumOfMargin}px)`
      }
    },
    changeValid (invalid, data) {
      this[data] = invalid
      this.$emit('changeValid', this.allInvalid)

      this.$el.style.height = this.$el.children[this.counter].offsetHeight + 'px'
    }
  },
  watch: {
    counter () {
      this.moveSlider()
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

.data {
  display: flex;
  flex-direction: column;
  gap: 15px;
  border: none;
  height: fit-content;
}

.input {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  color: var(--main-color);
  transition: 0.3s;

  &:focus-visible {
    outline: var(--outline-color) solid 4px;
  }
}

.select {
  @extend .input;

  option {
    margin: 5px 0;
    padding: 5px 0;
    border-radius: 5px;
    background: none;
    transition: 0.2s;

    &:checked {
      filter: grayscale(1);
      background: #cbcbcb;
    }
  }
}

.checkbox {
  position: relative;
  width: fit-content;
  padding-left: 25px;

  span {
    line-height: 1.3;

    &::before {
      position: absolute;
      left: 0;
      height: 20px;
      width: 20px;
      content: '';
      border-radius: 5px;
      background: var(--secondary-color);
    }

    &::after {
      position: absolute;
      left: 3px;
      opacity: 0;
      content: '✓';
      font-size: 20px;
      line-height: 1.1;
      color: var(--main-color);
      transition: 0.2s;
    }
  }

  input {
    display: none;

    &:checked ~ span::after {
      opacity: 1;
    }
  }
}

.invalid {
  margin: -11px 0 -5px;
  font-size: 14px;
  color: #fe4e3a;
}

.title {
  margin-bottom: 15px;
  font-weight: 700;
}

.data-wrapper {
  display: flex;
  @include adaptive(gap, 25, 15);
  transition: .6s;
}
</style>
