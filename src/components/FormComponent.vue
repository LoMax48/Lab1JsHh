<template>
    <div class="container">
        <h1>Заполните форму, чтобы сформировать резюме</h1>
        <div class="row my-3">
            <div class="col">
                <h4>Профессия:</h4>
                <input type="text" class="form-control" placeholder="Профессия" v-model="profession">
            </div>
            <div class="col">
                <h4>Город:</h4>
                <input type="text" class="form-control" placeholder="Город" v-model="city">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Ссылка на фото:</h4>
                <input type="text" class="form-control" placeholder="Ссылка на фото" v-model="image">
            </div>
            <div class="col">
                <h4>ФИО:</h4>
                <input type="text" class="form-control" placeholder="ФИО" v-model="FIO">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Телефон:</h4>
                <input type="text" class="form-control" placeholder="Телефон" 
                    v-model="phoneNumber" v-on:change="validatePhoneNumber">
                <div class="alert alert-danger" id="warning" role="alert">
                    Номер телефона может состоять только из цифр и иметь длину от 6 до 10 символов.
                </div>
            </div>
            <div class="col">
                <h4>E-mail:</h4>
                <input type="text" class="form-control" placeholder="E-mail" v-model="email">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Дата рождения:</h4>
                <input type="date" class="form-control" placeholder="Дата рождения" v-model="birthdate">
            </div>
            <div class="col">
                <h4>Образование:</h4>
                <select class="form-select" v-model="educationLevel">
                    <option>Среднее</option>
                    <option>Среднее профессиональное</option>
                    <option>Неоконченное высшее</option>
                    <option>Высшее</option>
                </select>
            </div>
        </div>
        <div class="row my-3" v-if="educationLevel === 'Среднее профессиональное' || educationLevel === 'Неоконченное высшее' || educationLevel === 'Высшее'">
            <div class="col">
                <h4>Учебное заведение</h4>
                <input type="text" class="form-control" placeholder="Учебное заведение" v-model="university">
            </div>
            <div class="col">
                <h4>Факультет</h4>
                <input type="text" class="form-control" placeholder="Факультет" v-model="faculty">
            </div>
        </div>
        <div class="row my-3" v-if="educationLevel === 'Среднее профессиональное' || 
            educationLevel === 'Неоконченное высшее' || educationLevel === 'Высшее'">
            <div class="col">
                <h4>Специализация</h4>
                <input type="text" class="form-control" placeholder="Специализация" v-model="specialization">
            </div>
            <div class="col">
                <h4>Год окончания</h4>
                <input type="number" class="form-control" placeholder="Год окончания" v-model="graduationYear">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h4>Желаемая зарплата:</h4>
                <input type="number" class="form-control" placeholder="Желаемая зарплата" v-model="salary">
            </div>
            <div class="col">
                <h4>Ключевые навыки:</h4>
                <input type="text" class="form-control" placeholder="Ключевые навыки" v-model="keySkills">
            </div>
        </div>
        <div class="row">
            <h4>О себе:</h4>
            <textarea rows="3" placeholder="О себе" v-model="about"></textarea>
        </div>
    </div>
    <div class="container">
        <h1>Ваше резюме</h1>
        <div class="row">
            <div class="col">
                <img :src="image">
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
        }
    },
    methods: {
        validatePhoneNumber() {
            var regex = /^[0-9]{6,10}$/i;
            if (regex.test(this.phoneNumber)) {
                document.getElementById("warning").style.display = "none";
            } else {
                document.getElementById("warning").style.display = "inherit";
            }
        }
    }
}
</script>