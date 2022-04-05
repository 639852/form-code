<template>
  <fieldset class="data">
    <legend class="title">Персональные данные:</legend>
    <input
      type="text"
      class="input"
      placeholder="Фамилия*"
      v-model.lazy="$v.lastName.$model"
    >
    <span class="invalid" v-if="$v.lastName.$error">
      <template v-if="!$v.lastName.validFormat">
        Поле должно содержать только кириллицу!
      </template>
      <template v-else>
        Поле обязательно для заполнения!
      </template>
    </span>
    <input
      type="text"
      class="input"
      placeholder="Имя*"
      v-model.lazy="$v.firstName.$model"
    >
    <span class="invalid" v-if="$v.firstName.$error">
      <template v-if="!$v.firstName.validFormat">
        Поле должно содержать только кириллицу!
      </template>
      <template v-else>
        Поле обязательно для заполнения!
      </template>
    </span>
    <input
      type="text"
      class="input"
      placeholder="Отчество"
      v-model.lazy="$v.patronymic.$model"
    >
    <span class="invalid" v-if="$v.patronymic.$error">
      Поле должно содержать только кириллицу!
    </span>
    <input
      type="text"
      class="input"
      placeholder="Дата рождения*"
      v-model.lazy="$v.birthday.$model"
    >
    <span class="invalid" v-if="$v.birthday.$error">
      Дата должна быть в формате ДД.ММ.ГГГГ!
    </span>
    <input
      type="tel"
      class="input"
      placeholder="Номер телефона*"
      v-model.lazy="$v.tel.$model"
    >
    <span class="invalid" v-if="$v.tel.$error">
      <template v-if="!$v.tel.maxLength">
        Длина номера не должна превышать {{ $v.tel.$params.maxLength.max }} символов!
      </template>
      <template v-else-if="!$v.tel.validFormat">
        Неверный формат номера!
      </template>
    </span>
    <input type="text" class="input" placeholder="Пол">
    <select
      multiple
      class="select"
      size="5"
      v-model.lazy="$v.customerGroup.$model"
    >
      <optgroup label="Группа клиентов*">
        <option value="VIP">VIP</option>
        <option value="Проблемные">Проблемные</option>
        <option value="ОМС">ОМС</option>
      </optgroup>
    </select>
    <span class="invalid" v-if="$v.customerGroup.$error">
      Поле обязательно для заполнения!
    </span>
    <select class="select">
      <optgroup label="Лечащий врач">
        <option value="">Выберите врача:</option>
        <option value="Иванов">Иванов</option>
        <option value="Захаров">Захаров</option>
        <option value="Чернышева">Чернышева</option>
      </optgroup>
    </select>
    <label class="checkbox">
      <input type="checkbox">
      <span>Не отправлять СМС.</span>
    </label>
  </fieldset>
</template>

<script>
import { required, maxLength } from 'vuelidate/lib/validators'

export default {
  data () {
    return {
      firstName: '',
      lastName: '',
      patronymic: '',
      birthday: '',
      tel: '',
      customerGroup: []
    }
  },
  validations: {
    firstName: {
      required,
      validFormat: value => /^[а-яё]*$/i.test(value)
    },
    lastName: {
      required,
      validFormat: value => /^[а-яё]*$/i.test(value)
    },
    patronymic: {
      validFormat: value => /^[а-яё]*$/i.test(value)
    },
    birthday: {
      required,
      validDate: value => /(0?[1-9]|[12][0-9]|3[01]).(0?[1-9]|1[012]).((19|20)\d\d)/.test(value)
    },
    tel: {
      required,
      maxLength: maxLength(11),
      validFormat: value => /^((\+7|7)+([0-9]){10})$/.test(value)
    },
    customerGroup: { required }
  },
  updated () {
    this.$emit('changeValid', this.$v.$invalid, 'personal')
  }
}
</script>
