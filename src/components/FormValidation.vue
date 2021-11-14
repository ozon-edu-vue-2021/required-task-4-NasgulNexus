<template>
  <form class="form" @submit.prevent="someAction()">
    <h2>Личные данные</h2>
    <div>
      <div>
        <label for="surname">Фамилия</label>
        <input v-model="surname" id="surname" @blur="$v.surname.$touch()" />
        <div v-if="$v.surname.$error" class="error">
          Фамилия должно содержать только Русские буквы
        </div>
      </div>
      <div>
        <label for="name">Имя</label>
        <input v-model="name" id="name" @blur="$v.name.$touch()" />
        <div v-if="$v.name.$error" class="error">
          Имя должно содержать только Русские буквы
        </div>
      </div>
      <div>
        <label for="patronymic">Отчество</label>
        <input
          v-model="patronymic"
          id="patronymic"
          @blur="$v.patronymic.$touch()"
        />
        <div v-if="$v.patronymic.$error" class="error">
          Отчество должно содержать только Русские буквы
        </div>
      </div>
      <div>
        <label for="age">Дата рождения</label>
        <input
          v-model="age"
          placeholder="дд.мм.гггг"
          id="age"
          @blur="$v.age.$touch()"
        />
        <div v-if="!isAge || $v.age.$error" class="error">
          Дата рождения должна быть в формате ДД.ММ.ГГГГ
        </div>
      </div>
      <div>
        <label for="email">E-mail</label>
        <input v-model="email" id="email" @blur="$v.email.$touch()" />
        <div v-if="$v.email.$error" class="error">
          Почта должна быть в виде myinbox@inbox.adreesInbox
        </div>
      </div>
      <div>
        <span>Пол</span>
        <br />
        <input
          v-model="pickedGender"
          type="radio"
          value="Men"
          id="Men"
          class="radio"
        />
        <label for="Men">Mужской</label>
        <input
          v-model="pickedGender"
          type="radio"
          value="Woman"
          id="Woman"
          class="radio"
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
      <div>
        <label for="passportSeries">Серия</label>
        <input
          v-model="passportSeries"
          id="passportSeries"
          @blur="$v.passportSeries.$touch()"
        />
        <div v-if="$v.passportSeries.$error" class="error">
          Серия паспорта должна быть из 4 цифр
        </div>
      </div>
      <div>
        <label for="passportID">Номер</label>
        <input
          v-model.number="passportID"
          id="passportID"
          @blur="$v.passportID.$touch()"
        />
        <div v-if="$v.passportID.$error" class="error">
          Номер паспорта должен быть из 6 цифр
        </div>
      </div>
      <div>
        <label for="dateOfIssueOfPassport">Дата выдачи</label>
        <input
          v-model="dateOfIssueOfPassport"
          placeholder="дд.мм.гггг"
          id="dateOfIssueOfPassport"
          @blur="$v.dateOfIssueOfPassport.$touch()"
        />
        <div v-if="$v.dateOfIssueOfPassport.$error" class="error">
          Дата выдачи паспорта должна быть в формате ДД.ММ.ГГГГ
        </div>
      </div>
    </div>
    <div v-else>
      <span>
        Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan
      </span>
      <div>
        <label for="surnameInLatin">Фамилия на латинице</label>
        <input
          v-model="surnameInLatin"
          id="surnameInLatin"
          @blur="$v.surnameInLatin.$touch()"
        />
        <div v-if="$v.surnameInLatin.$error" class="error">
          Фамилия должно содержать только латинские буквы
        </div>
      </div>
      <div>
        <label for="nameInLatin">Имя на латинице</label>
        <input
          v-model="nameInLatin"
          id="nameInLatin"
          @blur="$v.nameInLatin.$touch()"
        />
        <div v-if="$v.nameInLatin.$error" class="error">
          Имя должно содержать только латинские буквы
        </div>
      </div>
      <div>
        <label for="foreignPassportNumber">Номер</label>
        <input
          v-model.number="foreignPassportNumber"
          id="foreignPassportNumber"
          @blur="$v.foreignPassportNumber.$touch()"
        />
        <div v-if="$v.foreignPassportNumber.$error" class="error">
          Должны быть только цифры
        </div>
      </div>
      <div>
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
      <div>
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
        v-model="pickedNewSurnameOrName"
        type="radio"
        value="Yes"
        class="radio"
      />
      <label>Да</label>
      <input
        v-model="pickedNewSurnameOrName"
        type="radio"
        value="No"
        class="radio"
      />
      <label>Нет</label>
    </div>
    <div v-if="pickedNewSurnameOrName === 'Yes'">
      <div>
        <label for="formerSurname">Фамилия</label>
        <br />
        <input
          v-model="formerSurname"
          id="formerSurname"
          @blur="$v.formerSurname.$touch()"
        />
        <div v-if="$v.formerSurname.$error" class="error">
          Фамилия должно содержать только Русские буквы
        </div>
      </div>
      <div>
        <label for="formerName">Имя</label>
        <input
          v-model="formerName"
          id="formerName"
          @blur="$v.formerName.$touch()"
        />
        <div v-if="$v.formerName.$error" class="error">
          Имя должно содержать только Русские буквы
        </div>
      </div>
    </div>
    <button
      type="submit"
      :disabled="!isComplete || $v.$anyError === true || !isAge"
    >
      Отправить
    </button>
  </form>
</template>

<script>
import { required, maxLength, minLength } from "vuelidate/lib/validators";
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
      allcitizenships: citizenships,
      allpassportTypes: passportTypes,
      allData: {},
      time: moment().format("DDMMYYYY")
    };
  },
  computed: {
    isAge() {
      if (this.age) {
        if (
          parseInt(this.time.slice(4), 10) < parseInt(this.age.slice(6), 10)
        ) {
          return false;
        } else if (
          parseInt(this.time.slice(4), 10) > parseInt(this.age.slice(6), 10)
        ) {
          return true;
        } else if (
          parseInt(this.time.slice(2, 4), 10) <
          parseInt(this.age.slice(3, 5), 10)
        ) {
          return false;
        } else if (
          parseInt(this.time.slice(2, 4), 10) >
          parseInt(this.age.slice(3, 5), 10)
        ) {
          return true;
        } else if (
          parseInt(this.time.slice(0, 2), 10) >=
          parseInt(this.age.slice(0, 2), 10)
        ) {
          return true;
        } else {
          return false;
        }
      } else {
        return true;
      }
    },
    isComplete() {
      return this.surname &&
        this.name &&
        this.age &&
        this.patronymic &&
        this.pickedGender &&
        this.email &&
        this.selectedCountry === "Russia"
        ? this.passportID &&
          this.dateOfIssueOfPassport &&
          this.passportSeries &&
          this.pickedNewSurnameOrName == "Yes"
          ? this.formerSurname && this.formerName
          : this.pickedNewSurnameOrName == "No"
        : this.surnameInLatin &&
          this.foreignPassportNumber &&
          this.countryOfIssueOfThePassport &&
          this.typeOfPassport &&
          this.pickedNewSurnameOrName == "Yes"
        ? this.formerSurname && this.formerName
        : this.pickedNewSurnameOrName == "No";
    }
  },
  methods: {
    someAction() {
      //Личные данные
      this.allData.surname = this.surname;
      this.allData.name = this.name;
      this.allData.age = this.age;
      this.allData.patronymic = this.patronymic;
      this.allData.pickedGender = this.pickedGender;
      this.allData.email = this.email;
      //Паспортные данные
      this.allData.selectedCountry = this.selectedCountry;
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
      minLength: minLength(6),
      validFormat: val => /\d/.test(val)
    },
    passportSeries: {
      required,
      maxLength: maxLength(4),
      minLength: minLength(4),
      validFormat: val => /\d/.test(val)
    },
    dateOfIssueOfPassport: {
      required,
      validDate: val => moment(val, "DD.MM.YYYY", true).isValid()
    },
    email: {
      required,
      validFormat: val =>
        /[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/i.test(
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
  padding: 20px;
  border-radius: 30px;
  background: white;
}
input {
  width: 100%;
  height: 40px;
  padding: 5px;
  margin-bottom: 10px;
  border-radius: 5px;
  font-size: 18px;
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
  margin: 10px;
  border-radius: 10px;
  font-size: 18px;
  background-color: rgba(88, 121, 230, 0.192);
}
select {
  width: 100%;
  height: 40px;
  padding: 9px;
  margin-bottom: 10px;
  border-radius: 5px;
  font-size: 18px;
}
</style>
