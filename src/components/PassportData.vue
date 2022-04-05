<template>
  <fieldset class="data">
    <legend class="title">Паспортные данные:</legend>
    <select class="select" v-model.lazy="$v.documentType.$model">
      <optgroup label="Тип документа*">
        <option value="">Выберите тип документа:</option>
        <option value="passport">Паспорт</option>
        <option value="certificate">Свидетельство о рождении</option>
        <option value="license">Вод. удостоверение</option>
      </optgroup>
    </select>
    <span class="invalid" v-if="$v.documentType.$error">
      Поле обязательно для заполнения!
    </span>
    <input type="text" class="input" placeholder="Серия">
    <input
      type="text"
      class="input"
      placeholder="Номер*"
      v-model.lazy="$v.number.$model"
    >
    <span class="invalid" v-if="$v.number.$error">
      Номер паспорта должен быть в формате 567890!
    </span>
    <input type="text" class="input" placeholder="Кем выдан">
    <input
      type="text"
      class="input"
      placeholder="Дата выдачи*"
      v-model.lazy="$v.date.$model"
    >
    <span class="invalid" v-if="$v.date.$error">
      Дата должна быть в формате ДД.ММ.ГГГГ!
    </span>
  </fieldset>
</template>

<script>
import { required } from 'vuelidate/lib/validators'

export default {
  data () {
    return {
      documentType: '',
      number: '',
      date: ''
    }
  },
  validations: {
    documentType: { required },
    number: {
      required,
      validFormat: value => /^\d{6}$/.test(value)
    },
    date: {
      required,
      validDate: value => /(0?[1-9]|[12][0-9]|3[01]).(0?[1-9]|1[012]).((19|20)\d\d)/.test(value)
    }
  },
  updated () {
    this.$emit('changeValid', this.$v.$invalid, 'passport')
  }
}
</script>
