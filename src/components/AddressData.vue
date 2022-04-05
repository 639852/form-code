<template>
  <fieldset class="data">
    <legend class="title">Адресные данные:</legend>
    <input
      type="text"
      class="input"
      placeholder="Индекс"
      v-model.lazy="$v.index.$model"
    >
    <span class="invalid" v-if="$v.index.$error">
      Поле должно содержать только цифры!
    </span>
    <input type="text" class="input" placeholder="Страна">
    <input type="text" class="input" placeholder="Область">
    <input
      type="text"
      class="input"
      placeholder="Город*"
      v-model.lazy="$v.city.$model"
    >
    <span class="invalid" v-if="$v.city.$error">
      <template v-if="!$v.city.validFormat">
        Поле должно содержать только буквы!
      </template>
      <template v-else>
        Поле обязательно для заполнения!
      </template>
    </span>
    <input type="text" class="input" placeholder="Улица">
    <input type="text" class="input" placeholder="Дом">
  </fieldset>
</template>

<script>
import { required } from 'vuelidate/lib/validators'

export default {
  data () {
    return {
      index: '',
      city: ''
    }
  },
  validations: {
    index: {
      validFormat: value => /^[0-9]*$/.test(value)
    },
    city: {
      required,
      validFormat: value => /^[а-яёa-z]*$/i.test(value)
    }
  },
  updated () {
    this.$emit('changeValid', this.$v.$invalid, 'address')
  }
}
</script>
