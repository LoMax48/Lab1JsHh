<template>
    <div class="container">
        <h1>Заполните форму, чтобы сформировать резюме</h1>
        <div class="row my-3">
            <div class="col">
                <h4>ФИО:</h4>
                <InputComponent id="FIO" inputType="text" class="form-control" v-model="FIO" />
            </div>
            <div class="col">
                <h4>Дата рождения:</h4>
                <InputComponent id="birthdate" inputType="datetime-local" class="form-control" v-model="birthdate" />
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Ссылка на фото:</h4>
                <InputComponent id="image" inputType="text" class="form-control" v-model="image" />
            </div>
            <div class="col">
                <h4>Город проживания:</h4>
                <InputCityComponent id="city" v-model="city" />
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Телефон:</h4>
                <InputPhoneNumberComponent id="phone" inputType="tel" class="form-control" v-model="phoneNumber" />
            </div>
            <div class="col">
                <h4>E-mail:</h4>
                <InputComponent id="email" inputType="text" class="form-control" v-model="email" />
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Образование:</h4>
                <InputEducationComponent :citySearch="city" id="education" class="form-control" v-model="education" :educationFromDB="education" />
            </div>
            <div class="col">
                <h4>Профессия:</h4>
                <InputComponent id="profession" inputType="text" class="form-control" v-model="profession" />
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Желаемая зарплата:</h4>
                <InputComponent id="salary" inputType="number" class="form-control" v-model="salary" />    
            </div>
            <div class="col">
                <h4>Ключевые навыки:</h4>
                <InputComponent id="keySkills" inputType="text" class="form-control" v-model="keySkills" />
            </div>
        </div>
        <div class="row">
            <h4>О себе:</h4>
            <textarea rows="3" placeholder="О себе" v-model="about"></textarea>
        </div>
        <div class="row">
            <h4>Статус резюме</h4>
            <div id="v-model-select">
                <select class="form-select" v-model="statusResume">
                    <option selected>Новый</option>
                    <option>Назначено собеседование</option>
                    <option>Принят</option>
                    <option>Отказ</option>
                </select>
          </div>
        </div>
        <button @click="sendData" class="btn btn-primary" type="submit">
            Отправить
        </button>
    </div>
    <div class="container">
        <h1>Ваше резюме</h1>
        <div class="row">
            <div class="col">
                <img id="photo" :src="image">
            </div>
            <div class="col">
                <h4>ФИО: {{ FIO }}</h4>
                <h4>Профессия: {{ profession }}</h4>
                <h4>Город: {{ city }}</h4>
                <h4>Телефон: {{ phoneNumber }}</h4>
                <h4>Почта: {{ email }}</h4>
                <h4>Дата рождения: {{ birthdate }}</h4>
                <h4>Образование: {{ educationLevel }}</h4>
                <div v-if="educationLevel === 'Среднее профессиональное' || 
                    educationLevel === 'Неоконченное высшее' || educationLevel === 'Высшее'">
                    <h4>Учебное заведение: {{ university }}</h4>
                    <h4>Факультет: {{ faculty }}</h4>
                    <h4>Специализация: {{ specialization }}</h4>
                    <h4>Год окончания: {{ graduationYear }}</h4>
                </div>
                <h4>Желаемая зарплата: {{ salary }}</h4>
                <h4>Ключевые навыки: {{ keySkills }}</h4>
                <h4>О себе: {{ about }}</h4>
            </div>
        </div>
        <button type="button" class="btn btn-success my-3">Создать резюме</button>
    </div>
</template>

<script>
import InputComponent from "./inputs/InputComponent.vue";
import InputCityComponent from "./inputs/InputCityComponent.vue";
import InputEducationComponent from "./inputs/InputEducationComponent.vue";
import InputPhoneNumberComponent from "./inputs/InputPhoneNumberComponent.vue";
import axios from "axios";

export default {
    name: "FormComponent",
    data() {
        return {
            profession: null,
            city: null,
            image: null,
            FIO: null,
            phoneNumber: null,  
            email: null,
            birthdate: null,
            educationLevel: null,
            university: null,
            faculty: null,
            specialization: null,
            graduationYear: null,
            salary: null,
            keySkills: null,
            about: null,
            outPutDataEducation: null,
            sendDataToAPI: {},
            statusResume: "Новый",
            loaded: false
        }
    },
    components: {
        InputComponent,
        InputCityComponent,
        InputEducationComponent,
        InputPhoneNumberComponent
    },
    created() {
    console.log(this.$router)
    if (this.$router.currentRoute._value.params.id != undefined ) {
      console.log(this.$router.currentRoute._value.params.id);
      axios
        .get("http://localhost:8080/api/cv/"+this.$router.currentRoute._value.params.id)
        .then((response) => {
          console.log(response);
          this.FIO = response.data.FIO;
          this.phoneNumber = response.data.phone;
          this.email = response.data.email;
          this.birthdate = response.data.birthdate.substr(0, 16)
          this.education = {
              selectedEducation: response.data.education.selectedEducation,
              university: response.data.education.university,
              faculty: response.data.education.faculty,
              specialization: response.data.education.specialization,
              yearOfGraduation: response.data.education.yearOfGraduation,
              additionalEducationCount: response.data.education.additionalEducationCount,
              additionalEducations: response.data.education.additionalEducations,
          },
          this.profession = response.data.profession
          this.city = response.data.city
          this.salary = response.data.desired_salary
          this.keySkills = response.data.key_skills
          this.about = response.data.about_yourself
          this.image = response.data.url_photo
          this.statusResume =  response.data.status_of_resume
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => ((this.loaded = true)));
    }
    this.loaded = true
  },
  watch: {
    url: function (value) {
      document.getElementById("photo").src = value;
    },
    education: {
      handler() {
        this.fullEducation()
      },
      deep:true
    }
    
  },
  methods: {
    sendData() {
      console.log(this.$router.currentRoute);
      console.log(this.sendDataToAPI)
      if (this.$router.currentRoute._value.path == "/new" ) {
        axios
          .post("http://localhost:8080/api/cv/add", {
            FIO: this.FIO,
            phone: this.phoneNumber,
            email: this.email,
            date_of_birth: this.birthdate,
            education: {
              selectedEducation: this.education.selectedEducation,
              university: this.education.university,
              faculty: this.education.faculty,
              specialization: this.education.specialization,
              yearOfGraduation: this.education.yearOfGraduation,
              additionalEducationCount: this.education.additionalEducationCount,
              additionalEducations: (this.education.additionalEducations),
            },
            profession: this.profession,
            city: this.city,
            desired_salary: this.salary,
            key_skills: this.keySkills,
            about_yourself: this.about,
            url_photo: this.image,
            status_of_resume: this.statusResume,
          })
          .then((response) => {
            console.log('ok');
            console.log(response);
          })
          .catch((error) => {
            console.log('err');
            console.log(error);
            this.errored = true;
          })
      } else {
        axios
          .put("http://localhost:8080/api/cv/"+this.$router.currentRoute._value.params.id+"/edit", {
            id: this.$router.currentRoute._value.params.id,
            full_name: this.FIO,
            phone: this.phoneNumber,
            email: this.email,
            date_of_birth: this.birthdate,
            education: {
              selectedEducation: this.education.selectedEducation,
              university: this.education.university,
              faculty: this.education.faculty,
              specialization: this.education.specialization,
              yearOfGraduation: this.education.yearOfGraduation,
              additionalEducationCount: this.education.additionalEducationCount,
              additionalEducations: (this.education.additionalEducations),
            },
            profession: this.profession,
            city: this.city,
            desired_salary: this.salary,
            key_skills: this.keySkills,
            about_yourself: this.about,
            url_photo: this.image,
            status_of_resume: this.statusResume,
          })
          .then((response) => {
            console.log('ok');
            console.log(response);
          })
          .catch((error) => {
            console.log('err');
            console.log(error);
            this.errored = true;
          })
      } 
    },
    fullEducation() {
      this.outPutDataEducation = ""
      if (this.education.yearOfGraduation != undefined) {
        this.outPutDataEducation += this.education.selectedEducation + " " + this.education.university + " " + this.education.faculty + " " + this.education.specialization + " " + this.education.yearOfGraduation + " ."
      }
      if (this.education.additionalEducationCount > 0) {
        this.education.additionalEducations.forEach(additionalEducation => {
          if (additionalEducation.selectedAdditionalEducation != "Среднее") {
            if (additionalEducation.city != " " && additionalEducation.university != " " && additionalEducation.faculty != " " && additionalEducation.specialization != " " && additionalEducation.yearOfGraduation!= " ") {
              this.outPutDataEducation += additionalEducation.city + " " +  additionalEducation.selectedAdditionalEducation + " " + additionalEducation.additionalUniversity + " " + additionalEducation.additionalFaculty + " " + additionalEducation.additionalSpecialization + " " + additionalEducation.additionalYearOfGraduation + " ."
            }
          } else {
            this.outPutDataEducation += additionalEducation.selectedAdditionalEducation
          }
        })
      }
      return this.outPutDataEducation
    }
  },
};
</script>