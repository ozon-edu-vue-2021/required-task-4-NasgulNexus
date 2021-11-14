<template>
  <form class="form" @submit.prevent="someAction()">
    <h2>Личные данные</h2>
    <div>
      <div>
        <label for="surname">Фамилия</label>
        <input v-model="formData.surname" id="surname" />
      </div>
      <div>
        <label for="name">Имя</label>
        <input v-model="formData.name" id="name" />
      </div>
      <div>
        <label for="patronymic">Отчество</label>
        <input v-model="formData.patronymic" id="patronymic" />
      </div>
      <div>
        <label for="age">Дата рождения</label>
        <input v-model="formData.age" id="age" placeholder="дд.мм.гггг" />
      </div>
      <div>
        <label for="email">E-mail</label>
        <input v-model="formData.email" id="email" />
      </div>
      <div>
        <span>Пол:</span>
        <br />
        <input
          class="radio"
          type="radio"
          value="Men"
          id="Men"
          v-model="formData.pickedGender"
        />
        <label for="Men">Mужской</label>
        <input
          class="radio"
          type="radio"
          value="Woman"
          id="Woman"
          v-model="formData.pickedGender"
        />
        <label for="Woman">Женский</label>
      </div>
    </div>
    <h2>Паспортные данные</h2>
    <span> Гражданство</span>
    <select v-if="allcitizenships.length" v-model="formData.selectedCountry">
      <option disabled>Выберите один из вариантов</option>
      <option v-for="index in allcitizenships" :key="index.id">
        {{ index.nationality }}
      </option>
    </select>
    <div v-else>Ничего не найдено {{ formData.selectedCountry }}</div>
    <div v-if="formData.selectedCountry === 'Russia'">
      <div>
        <label for="passportSeries">Серия</label>
        <input v-model="formData.passportSeries" id="passportSeries" />
      </div>
      <div>
        <label for="passportID">Номер</label>
        <input v-model.number="formData.passportID" id="passportID" />
      </div>
      <div>
        <label for="dateOfIssueOfPassport">Дата выдачи</label>
        <input
          v-model="formData.dateOfIssueOfPassport"
          id="dateOfIssueOfPassport"
          placeholder="дд.мм.гггг"
        />
      </div>
    </div>
    <div v-else>
      <span>
        Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan
      </span>
      <div>
        <label for="surnameInLatin">Фамилия на латинице</label>
        <input v-model="formData.surnameInLatin" id="surnameInLatin" />
      </div>
      <div>
        <label for="nameInLatin">Имя на латинице</label>
        <input v-model="formData.nameInLatin" id="nameInLatin" />
      </div>
      <div>
        <label for="foreignPassportNumber">Номер</label>
        <input
          v-model.number="formData.foreignPassportNumber"
          id="foreignPassportNumber"
        />
      </div>
      <div>
        <span>Страна выдачи</span>
      </div>
      <select
        v-if="allcitizenships.length"
        v-model="formData.countryOfIssueOfThePassport"
      >
        <option disabled>Выберите один из вариантов</option>
        <option v-for="index in allcitizenships" :key="index.id">
          {{ index.nationality }}
        </option>
      </select>
      <div v-else>Ничего не найдено</div>
      <div>
        <label for="typeOfPassport">Тип паспорта</label>
        <select
          v-if="allpassportTypes.length"
          v-model="formData.typeOfPassport"
        >
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
        v-model="formData.pickedNewSurnameOrName"
      />
      <label>Да</label>
      <input
        class="radio"
        type="radio"
        value="No"
        v-model="formData.pickedNewSurnameOrName"
      />
      <label>Нет</label>
    </div>
    <div v-if="formData.pickedNewSurnameOrName === 'Yes'">
      <div>
        <label for="formerSurname">Фамилия</label>
        <input v-model="formData.formerSurname" id="formerSurname" />
      </div>
      <div>
        <label for="formerName">Имя</label>
        <input v-model="formData.formerName" id="formerName" />
      </div>
    </div>
    <button type="submit">Отправить</button>
  </form>
</template>

<script>
import citizenships from "@/assets/data/citizenships.json";
import passportTypes from "@/assets/data/passport-types.json";
export default {
  data() {
    return {
      formData: {
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
        typeOfPassport: null,
        countryOfIssueOfThePassport: null,
        //Менял ли фамилию или имя
        pickedNewSurnameOrName: null,
        formerSurname: null,
        formerName: null
      },
      allcitizenships: citizenships,
      allpassportTypes: passportTypes
    };
  },
  methods: {
    someAction() {
      console.log(this.formData);
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
