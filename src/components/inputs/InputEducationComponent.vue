<template>
  <div>
    <select class="form-select" v-model="educationFull.selectedEducation">
      <option disabled value="">Выберите один из вариантов</option>
      <option v-for="item in items" v-bind:key="item">
        {{ item.value }}
      </option>
    </select>
  </div>
  <div v-if="fullEducation">
    <div class="mb-3">
      <label for="university" class="form-label">Университет</label>
      <InputComponent id="university" inputType="text" class="form-control" v-model="educationFull.university" />
      <p v-show="showHint">Возможно вы имели ввиду...</p>
      <div class="list-group" v-show="showHint">
        <button @click="insertSearch" type="button" class="list-group-item list-group-item-action" v-for="hint in hints" v-bind:key="hint">
          {{ hint }}
        </button>
      </div>
    </div>
    <div class="mb-3">
      <label for="faculty" class="form-label">Факультет</label>
      <InputComponent id="faculty" inputType="text" class="form-control" v-model="educationFull.faculty" />
    </div>
    <div class="mb-3">
      <label for="specialization" class="form-label">Специальность</label>
      <InputComponent id="specialization" inputType="text" class="form-control" v-model="educationFull.specialization" />
    </div>
    <div class="mb-3">
      <label for="yearOfGraduation" class="form-label">Год окончания</label>
      <InputComponent id="yearOfGraduation" inputType="text" class="form-control" v-model="educationFull.yearOfGraduation" />
    </div>
    <button class="btn btn-primary mb-2" @click="addNewEducation">
      Добавить ещё образование
    </button>
    <div v-if="educationFull.additionalEducationCount > 0">
      <div v-for="additionalEducationItem in educationFull.additionalEducations" v-bind:key="additionalEducationItem">
        <div>
          <select class="form-select" v-model="additionalEducationItem.selectedAdditionalEducation">
            <option disabled value="">Выберите один из вариантов</option>
            <option v-for="item in items" v-bind:key="item">
              {{ item.value }}
            </option>
          </select>
        </div>
        <div v-if="additionalEducationItem.selectedAdditionalEducation != 'Среднее'">
          <div class="mb-3">
            <label for="yearOfGraduation" class="form-label">Город</label>
            <InputComponent id="city" inputType="text" class="form-control" v-model="additionalEducationItem.city" />
          </div>
          <div class="mb-3">
            <label for="university" class="form-label">Университет</label>
            <InputComponent id="university" inputType="text" class="form-control" v-model="additionalEducationItem.additionalUniversity" />
          </div>
          <div class="mb-3">
            <label for="faculty" class="form-label">Факультет</label>
            <InputComponent id="faculty" inputType="text" class="form-control" v-model="additionalEducationItem.additionalFaculty" />
          </div>
          <div class="mb-3">
            <label for="specialization" class="form-label">Специальность</label>
            <InputComponent id="specialization" inputType="text" class="form-control" v-model="additionalEducationItem.additionalSpecialization" />
          </div>
          <div class="mb-3">
            <label for="yearOfGraduation" class="form-label">Год окончания</label>
            <InputComponent id="yearOfGraduation" inputType="text" class="form-control" v-model="additionalEducationItem.additionalYearOfGraduation" />
          </div>
          <button @click="deleteEducation(additionalEducationItem.number - 1)" type="button" class="btn btn-danger">
            Удалить
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import InputComponent from "./InputComponent.vue";
import jsonp from "jsonp";

export default {
  data() {
    return {
      educationFull: {
        selectedEducation: " ",
        university: " ",
        faculty: " ",
        specialization: " ",
        yearOfGraduation: " ",
        additionalEducationCount: 0,
        additionalEducations: [],
      },
      needCity: " ",
      showHint: false,
      hints: [],
      fullEducation: false,
      items: [
        { value: "Среднее" },
        { value: "Среднее специальное" },
        { value: "Неоконченное высшее" },
        { value: "Высшее" },
      ],
    };
  },
  components: {
    InputComponent,
  },
  name: "InputEducationComponent",
  props: ['modelValue', 'citySearch', 'educationFromDB'],
  created() {
    console.log(this.educationFromDB)
    if (this.educationFromDB != {}) {
      this.educationFull = this.educationFromDB;
      if (this.educationFull.selectedEducation != "Среднее") {
        this.fullEducation = true;
      } else {
        this.fullEducation = false;
      }
    }
  },
  watch: {
    educationFull: {
      handler(val, oldVal) {
        console.log(val.university);
        console.log(oldVal.university);
        if (val.selectedEducation != "Среднее") {
          this.fullEducation = true;
        } else {
          this.fullEducation = false;
        }
        if (this.educationFromDB == {}) {
          jsonp(
            `http://api.vk.com/method/database.getCities?country_id=1&q=${this.citySearch}&count=1&v=5.131&access_token=${process.env.VUE_APP_ACCESS_TOKEN}`,
            null,
            (err, data) => {
              if (err) {
                console.log(err);
              } else {
                this.needCity = data.response.items[0].id;
                jsonp(
                  `http://api.vk.com/method/database.getUniversities?city_id=${this.needCity}&country_id=1&q=${this.educationFull.university}&count=3&v=5.131&access_token=${process.env.VUE_APP_ACCESS_TOKEN}`,
                  null,
                  (err, data) => {
                    if (err) {
                      console.log(err);
                    } else {
                      console.log(data);
                      this.showHint = true;
                      this.hints = [];
                      data.response.items.forEach((element) => {
                        this.hints.push(element.title);
                      });
                    }
                  }
                );
              }
            }
          );
        }
        this.educationFull.university = val.university;
        this.educationFull.faculty = val.faculty;
        this.educationFull.specialization = val.specialization;
        this.educationFull.yearOfGraduation = val.yearOfGraduation;
        this.$emit("update:modelValue", this.educationFull);
      },
      deep: true,
    },
  },
  computed: {
    inputValue: {
      get() {
        return this.educationFull;
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  methods: {
    deleteEducation(index) {
      this.educationFull.additionalEducationCount--;
      this.educationFull.additionalEducations.splice(index, 1);
    },
    addNewEducation() {
      this.educationFull.additionalEducationCount += 1;
      this.educationFull.additionalEducations.push({
        number: this.educationFull.additionalEducationCount,
        city: " ",
        selectedAdditionalEducation: " ",
        additionalUniversity: " ",
        additionalFaculty: " ",
        additionalSpecialization: " ",
        additionalYearOfGraduation: " ",
      });
    },
    insertSearch(event) {
      this.educationFull.university = event.target.innerHTML;
    },
    insertSearchadditionalEducation(index, event) {
      this.educationFull.additionalEducations[index].university =
        event.target.innerHTML;
    },
  },
};
</script>