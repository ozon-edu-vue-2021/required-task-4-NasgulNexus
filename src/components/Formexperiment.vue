<template>
  <form class="form" @submit.prevent="someAction()">
    <h2>Личные данные</h2>
    <div>
      <div class="row">
        <label for="surname">Фамилия</label>
        <input v-model="surname" @blur="$v.surname.$touch()" id="surname" />
        <div class="error" v-if="$v.surname.$error">
          Фамилия должно содержать только Русские буквы
        </div>
      </div>
      <div class="row">
        <label for="name">Имя</label>
        <input v-model="name" id="name" @blur="$v.name.$touch()" />
        <div class="error" v-if="$v.name.$error">
          Имя должно содержать только Русские буквы
        </div>
      </div>
      <div class="row">
        <label for="patronymic">Отчество</label>
        <input
          v-model="patronymic"
          id="patronymic"
          @blur="$v.patronymic.$touch()"
        />
        <div class="error" v-if="$v.patronymic.$error">
          Отчество должно содержать только Русские буквы
        </div>
      </div>
      <div class="row">
        <label for="age">Дата рождения</label>
        <input
          v-model="age"
          id="age"
          placeholder="дд.мм.гггг"
          @blur="$v.age.$touch()"
        />
        <div class="error" v-if="$v.age.$error">
          Дата рождения должна быть в формате ДД.ММ.ГГГГ
        </div>
      </div>
      <div class="row">
        <label for="email">E-mail</label>
        <input v-model="email" id="email" @blur="$v.email.$touch()" />
        <div class="error" v-if="$v.email.$error">
          Почта должна быть в виде myinbox@inbox.adreesInbox
        </div>
      </div>
      <div class="row">
        <span>Пол</span>
        <br />
        <input
          class="radio"
          type="radio"
          value="Men"
          id="Men"
          v-model="pickedGender"
        />
        <label for="Men">Mужской</label>
        <input
          class="radio"
          type="radio"
          value="Woman"
          id="Woman"
          v-model="pickedGender"
        />
        <label for="Woman">Женский</label>
      </div>
    </div>
    <h2>Паспортные данные</h2>
    <span> Гражданство</span>
    <select v-if="allcitizenships.length" v-model="selectedCountry">
      <option disabled>Выберите один из вариантов</option>
      <option v-for="index in allcitizenships" :key="index.id">
        {{ index.nationality }}
      </option>
    </select>
    <div v-else>Ничего не найдено</div>
    <div v-if="selectedCountry === 'Russia'">
      <div class="row">
        <label for="passportSeries">Серия</label>
        <input
          v-model="passportSeries"
          id="passportSeries"
          @blur="$v.passportSeries.$touch()"
        />
        <div class="error" v-if="$v.passportSeries.$error">
          Серия паспорта должна быть из 4 цифр
        </div>
      </div>
      <div class="row">
        <label for="passportID">Номер</label>
        <input
          v-model.number="passportID"
          @blur="$v.passportID.$touch()"
          id="passportID"
        />
        <div class="error" v-if="$v.passportID.$error">
          Номер паспорта должен быть из 6 цифр
        </div>
      </div>
      <div class="row">
        <label for="dateOfIssueOfPassport">Дата выдачи</label>
        <input
          v-model="dateOfIssueOfPassport"
          id="dateOfIssueOfPassport"
          @blur="$v.dateOfIssueOfPassport.$touch()"
          placeholder="дд.мм.гггг"
        />
        <div class="error" v-if="$v.dateOfIssueOfPassport.$error">
          Дата выдачи паспорта должна быть в формате ДД.ММ.ГГГГ
        </div>
      </div>
    </div>
    <div v-else>
      <span>
        Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan
      </span>
      <div class="row">
        <label for="surnameInLatin">Фамилия на латинице</label>
        <input
          v-model="surnameInLatin"
          @blur="$v.surnameInLatin.$touch()"
          id="surnameInLatin"
        />
        <div class="error" v-if="$v.surnameInLatin.$error">
          Фамилия должно содержать только латинские буквы
        </div>
      </div>
      <div class="row">
        <label for="nameInLatin">Имя на латинице</label>
        <input
          v-model="nameInLatin"
          @blur="$v.nameInLatin.$touch()"
          id="nameInLatin"
        />
        <div class="error" v-if="$v.nameInLatin.$error">
          Имя должно содержать только латинские буквы
        </div>
      </div>
      <div class="row">
        <label for="foreignPassportNumber">Номер</label>
        <input
          v-model.number="foreignPassportNumber"
          @blur="$v.foreignPassportNumber.$touch()"
          id="foreignPassportNumber"
        />
        <div class="error" v-if="$v.foreignPassportNumber.$error">
          Должны быть только цифры
        </div>
      </div>
      <div class="row">
        <span>Страна выдачи</span>
      </div>
      <select
        v-if="allcitizenships.length"
        v-model="countryOfIssueOfThePassport"
      >
        <option disabled>Выберите один из вариантов</option>
        <option v-for="index in allcitizenships" :key="index.id">
          {{ index.nationality }}
        </option>
      </select>
      <div v-else>Ничего не найдено</div>
      <div class="row">
        <label for="typeOfPassport">Тип паспорта</label>
        <select v-if="allpassportTypes.length" v-model="typeOfPassport">
          <option disabled>Выберите один из вариантов</option>
          <option v-for="index in allpassportTypes" :key="index.id">
            {{ index.type }}
          </option>
        </select>
        <div v-else>Ничего не найдено</div>
      </div>
    </div>
    <div>
      <span>Меняли ли фамилию или имя</span>
      <br />
      <input
        class="radio"
        type="radio"
        value="Yes"
        v-model="pickedNewSurnameOrName"
      />
      <label>Да</label>
      <input
        class="radio"
        type="radio"
        value="No"
        v-model="pickedNewSurnameOrName"
      />
      <label>Нет</label>
    </div>
    <div v-if="pickedNewSurnameOrName === 'Yes'">
      <div class="row">
        <label for="formerSurname">Фамилия</label>
        <br />
        <input
          v-model="formerSurname"
          @blur="$v.formerSurname.$touch()"
          id="formerSurname"
        />
        <div class="error" v-if="$v.formerSurname.$error">
          Фамилия должно содержать только Русские буквы
        </div>
      </div>
      <div class="row">
        <label for="formerName">Имя</label>
        <input
          v-model="formerName"
          id="formerName"
          @blur="$v.formerName.$touch()"
        />
        <div class="error" v-if="$v.formerName.$error">
          Имя должно содержать только Русские буквы
        </div>
      </div>
    </div>
    <button type="submit">Отправить</button>
  </form>
</template>

<script>
import { required, maxLength } from "vuelidate/lib/validators";
import moment from "moment";
import citizenships from "@/assets/data/citizenships.json";
import passportTypes from "@/assets/data/passport-types.json";
export default {
  data() {
    return {
      //Личные данные
      surname: null,
      name: null,
      age: null,
      patronymic: null,
      pickedGender: null,
      email: null,
      //Паспортные данные
      selectedCountry: "Russia",
      //Когда гражданство Россия
      passportID: null,
      dateOfIssueOfPassport: null,
      passportSeries: null,
      //Другое гражданство
      surnameInLatin: null,
      nameInLatin: null,
      foreignPassportNumber: null,
      countryOfIssueOfThePassport: null,
      typeOfPassport: null,
      //Менял ли фамилию или имя
      pickedNewSurnameOrName: null,
      formerSurname: null,
      formerName: null,
      //Вспомогательные данные
      isDropdownOpen: false,
      allcitizenships: citizenships,
      allpassportTypes: passportTypes,
      allData: {}
    };
  },
  methods: {
    someAction() {
      //Личные данные
      this.allData.surname = this.surname;
      this.allData.name = this.name;
      this.allData.age = this.age;
      this.allData.patronymic = this.patronymic;
      this.allData.pickedGender = this.pickedGende;
      this.allData.email = this.email;
      //Паспортные данные
      this.allData.selectedCountry = this.selectedCountry;
      this.allData.selectedCountry2 = this.selectedCountry2;
      //Когда гражданство Россия
      this.allData.passportID = this.passportID;
      this.allData.dateOfIssueOfPassport = this.dateOfIssueOfPassport;
      this.allData.passportSeries = this.passportSeries;
      //Другое гражданство
      this.allData.surnameInLatin = this.surnameInLatin;
      this.allData.nameInLatin = this.nameInLatin;
      this.allData.foreignPassportNumber = this.foreignPassportNumber;
      this.allData.typeOfPassport = this.typeOfPassport;
      this.allData.countryOfIssueOfThePassport =
        this.countryOfIssueOfThePassport;
      //Менял ли фамилию или имя
      this.allData.pickedNewSurnameOrName = this.pickedNewSurnameOrName;
      this.allData.formerSurname = this.formerSurname;
      this.allData.formerName = this.formerName;
      console.log(this.allData);
    },
    hideDropdown() {
      this.isDropdownOpen = false;
    },
    onCountryCliked(selectedCountry) {
      this.selectedCountry2 = selectedCountry;
      this.hideDropdown();
    }
  },
  validations: {
    age: {
      required,
      validDate: val => moment(val, "DD.MM.YYYY", true).isValid()
    },
    name: {
      required,
      alpha: val => /^[а-яё]*$/i.test(val)
    },
    patronymic: {
      required,
      alpha: val => /^[а-яё]*$/i.test(val)
    },
    surname: {
      required,
      alpha: val => /^[а-яё]*$/i.test(val)
    },
    passportID: {
      required,
      maxLength: maxLength(6),
      validFormat: val => /\d/.test(val)
    },
    passportSeries: {
      required,
      maxLength: maxLength(4),
      validFormat: val => /\d/.test(val)
    },
    dateOfIssueOfPassport: {
      required,
      validDate: val => moment(val, "DD.MM.YYYY", true).isValid()
    },
    email: {
      required,
      validFormat: val =>
        /[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/.test(
          val
        )
    },
    formerSurname: {
      required,
      alpha: val => /^[а-яё]*$/i.test(val)
    },
    formerName: {
      required,
      alpha: val => /^[а-яё]*$/i.test(val)
    },
    surnameInLatin: {
      required,
      alpha: val => /^[a-z]*$/i.test(val)
    },
    nameInLatin: {
      required,
      alpha: val => /^[a-z]*$/i.test(val)
    },
    foreignPassportNumber: {
      required,
      validFormat: val => /\d/.test(val)
    }
  }
};
</script>

<style scoped>
.form {
  max-width: 600px;
  margin: auto;
  background: white;
  border-radius: 30px;
  padding: 20px;
}
input {
  width: 100%;
  height: 40px;
  border-radius: 5px;
  padding: 5px;
  font-size: 18px;
  margin-bottom: 10px;
}
label {
  font-size: 18px;
  color: rgba(0, 0, 0, 0.5);
}
span {
  font-size: 18px;
}
.radio {
  width: 20px;
  height: 20px;
  margin-left: 55px;
}
.error {
  color: red;
}
button {
  font-size: 18px;
  background-color: rgba(88, 121, 230, 0.192);
  border-radius: 10px;
  margin: 10px;
}
select {
  width: 100%;
  height: 40px;
  border-radius: 5px;
  padding: 9px;
  font-size: 18px;
  margin-bottom: 10px;
}
</style>
