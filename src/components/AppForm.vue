<template>
  <div class="wrapper">
    <div class="title">{{ title }}</div>
    <form class="form" id="form">
      <div class="form-control" :class="{ invalid: v$.lastName.$error }">
        <label for="lastName">Фамилия*</label>
        <input
          type="text"
          id="lastName"
          v-model="lastName"
          @blur="v$.lastName.$touch"
          :class="v$.lastName.$error ? 'invalid' : ''"
        />

        <small v-if="v$.lastName.$error">Введите фамилию</small>
      </div>

      <div class="form-control" :class="{ invalid: v$.firstName.$error }">
        <label for="name">Имя*</label>
        <input
          type="text"
          id="name"
          v-model="firstName"
          @blur="v$.firstName.$touch"
        />
        <small v-if="v$.firstName.$error">Введите имя</small>
      </div>

      <div class="form-control">
        <label for="middleName">Отчество</label>
        <input type="text" id="middleName" v-model="middleName" />
      </div>

      <div class="form-control" :class="{ invalid: v$.DateOfBirth.$error }">
        <label for="DateOfBirth">Дата рождения*</label>
        <input
          type="date"
          id="DateOfBirth"
          v-model="DateOfBirth"
          @blur="v$.DateOfBirth.$touch"
        />
        <small v-if="v$.DateOfBirth.$error">Укажите дату рождения</small>
      </div>

      <div class="form-control" :class="{ invalid: v$.phone.$error }">
        <label for="phone">Номер телефона*</label>
        <input type="tel" id="phone" v-model="phone" @blur="v$.phone.$touch" />
        <small v-if="v$.phone.$error">Введите номер телефона</small>
      </div>

      <div class="form-control">
        <div class="title__btn">Пол</div>
        <button
          :class="gender == 'man' ? 'btn active' : 'btn'"
          @click.prevent="gender = 'man'"
        >
          Муж
        </button>
        <button
          class="btn"
          @click.prevent="gender = 'female'"
          :class="gender == 'female' ? 'btn active' : 'btn'"
        >
          Жен
        </button>
      </div>

      <div class="form-control" :class="{ invalid: v$.clientGroup.$error }">
        <label for="clientGroup">Группа клиентов*</label>
        <select
          id="clientGroup"
          class="select"
          v-model="clientGroup"
          multiple
          size="3"
          @blur="v$.clientGroup.$touch"
        >
          <option value="VIP">VIP</option>
          <option value="problems">Проблемные</option>
          <option value="oms">ОМС</option>
        </select>
        <small v-if="v$.clientGroup.$error"> Выберите группу </small>
      </div>

      <div class="form-control">
        <label for="doctor">Лечащий врач</label>
        <select id="doctor" class="select" v-model="doctor">
          <option value="doctor1">Иванов</option>
          <option value="doctor2">Захаров</option>
          <option value="doctor3">Чернышева</option>
        </select>
      </div>

      <div class="form-control">
        <label for="sms">Не отправлять СМС</label>
        <input
          v-model="sms"
          @click="sms = !sms"
          class="checkbox"
          type="checkbox"
          name="sms"
          id="sms"
        />
      </div>

      <div class="address">
        <div class="block">
          <div class="form-control">
            <label for="index">Индекс</label>
            <input type="text" id="index" v-model="index" />
          </div>

          <div class="form-control">
            <label for="country">Страна</label>
            <input type="text" id="country" v-model="country" />
          </div>

          <div class="form-control">
            <label for="region">Область</label>
            <input type="text" id="region" v-model="region" />
          </div>
        </div>
        <div class="block">
          <div class="form-control" :class="{ invalid: v$.city.$error }">
            <label for="city">Город*</label>
            <input
              type="text"
              id="city"
              v-model="city"
              @blur="v$.city.$touch"
            />
            <small v-if="v$.city.$error">Необходимо указать город</small>
          </div>
          <div class="form-control">
            <label for="street">Улица</label>
            <input type="text" id="street" v-model="street" />
          </div>
          <div class="form-control">
            <label for="house">Дом</label>
            <input type="text" id="house" v-model="house" />
          </div>
        </div>
      </div>

      <div class="passport">
        <div class="form-control" :class="{ invalid: v$.document.$error }">
          <label for="document">Тип документа*</label>
          <select
            id="document"
            class="select"
            v-model="document"
            @blur="v$.document.$touch"
          >
            <option value="passport">Паспорт</option>
            <option value="birthCertificate">Свидетельство о рождении</option>
            <option value="driverLicense">Вод. удостоверение</option>
          </select>
          <small v-if="v$.document.$error">Выберите тип документа</small>
        </div>

        <div class="row">
          <div class="block">
            <div class="form-control">
              <label for="series">Серия</label>
              <input type="text" id="series" v-model="series" />
            </div>
          </div>

          <div class="block">
            <div class="form-control">
              <label for="room">Номер</label>
              <input type="text" id="room" v-model="room" />
            </div>
          </div>
        </div>

        <div class="form-control">
          <label for="issuedBy">Кем выдан</label>
          <input type="text" id="issuedBy" v-model="issuedBy" />
        </div>

        <div class="form-control" :class="{ invalid: v$.dateIssue.$error }">
          <label for="dateIssue">Дата выдачи*</label>
          <input
            type="date"
            id="dateIssue"
            v-model="dateIssue"
            @blur="v$.dateIssue.$touch"
          />
          <small v-if="v$.dateIssue.$error"> Укажите дату выдачи </small>
        </div>
      </div>
      <button
        class="btn-submit"
        @click.prevent="submit"
        type="submit"
        :disabled="submitStatus === 'PENDING'"
      >
        Создать
      </button>
    </form>
  </div>
</template>

<script>
import { required, minLength } from "@vuelidate/validators";
import { useVuelidate } from "@vuelidate/core";

export default {
  name: "HelloWorld",
  props: {
    title: String,
  },

  setup() {
    return {
      v$: useVuelidate(),
    };
  },

  data() {
    return {
      firstName: null,
      lastName: null,
      middleName: null,
      DateOfBirth: null,
      phone: null,
      gender: null,
      clientGroup: [],
      doctor: null,
      sms: false,

      index: null,
      country: null,
      region: null,
      city: null,
      street: null,
      house: null,

      document: null,
      series: null,
      room: null,
      issuedBy: null,
      dateIssue: null,

      submitStatus: null,
    };
  },
  validations() {
    return {
      lastName: { required },
      firstName: { required },
      middleName: { required: false },
      DateOfBirth: { required },
      phone: { required, minLength: minLength(11) },
      gender: { required: false },
      clientGroup: { required },
      doctor: { required: false },
      sms: { required: false },

      index: { required: false },
      country: { required: false },
      region: { required: false },
      city: { required },
      street: { required: false },
      house: { required: false },

      document: { required },
      series: { required: false },
      room: { required: false },
      issuedBy: { required: false },
      dateIssue: { required },
    };
  },
  methods: {
    submit() {
      this.v$.$touch();
      console.log(this.v$.$touch());
      if (this.v$.$invalid) {
        alert("Пожалуйста, заполните все обязательные поля!");
      } else {
        this.submitStatus = "PENDING";
        setTimeout(() => {
          alert("Форма успешно создана!");
          this.submitStatus = "OK";
        }, 500);
      }
    },
  },
};
</script>

<style lang="sass">
.wrapper
  max-width: 1230px
  padding: 15px 15px
  margin: 0 auto
  background-color: #165FC6
  border: 1px solid black

.title
  color: #FFFFFF
  font-size: 1.5rem
  margin-bottom: 1rem

.form
  margin: 0 auto
  padding: 15px 15px;
  color: #FFFFFF;
  width: 400px

.form-control
  position: relative;
  margin-bottom: 1.5rem;

.form-control input,
.form-control select,
.form-control textarea
  font-weight: 300;
  margin: 0;
  outline: none;
  border: 2px solid #ccc;
  display: block;
  width: 100%;
  padding: 8px 0px;
  border-radius: 3px;
  font-size: 1rem;
  resize: vertical;
  background-color: #FFFFFF;


.form-control select
  text-align: center

.checkbox
  width: 20px
  height: 20px
  cursor: pointer
  background-color: red

.form-control small
  color: #e53935;
  position: absolute
  left: 0

.form-control.invalid input,
.form-control.invalid select
  border-color: #e53935;

.form-control label
  display: block;
  margin: 0 0 0.3rem 0.3rem;
  font-weight: 600;


.form-control input:active,
.form-control textarea:focus,
.form-control input:focus
  transition: border 0.22s;
  border: 2px solid #42b983;

.title__btn
  font-size: 1rem;
.btn
  margin: 5px
  padding: 5px 15px
  border: 1px solid black;
  border-radius: 8px
  background-color: #FFFFFF;
  cursor: pointer

.btn.active
  border: 2px solid #42b983

.address
  display: flex
.block
  margin: 0 5px

.row
  display: flex

.btn-submit
  cursor: pointer
  padding: 5px 0px
  width: 100%
  background-color: #42b983
  border: 1px solid black

@media screen and(max-width: 425px)
  .title
    font-size: 1rem
    margin-bottom: 1rem
  .form
    width: 250px
    padding: 0;
  .form-control label
    font-weight: 400
    font-size: .9rem
  .form-control input,
  .form-control select,
  .form-control textarea
    font-size: .8rem
  .address
    flex-direction: column
</style>
