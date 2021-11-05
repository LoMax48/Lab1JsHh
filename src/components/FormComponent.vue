<template>
    <div class="container">
        <h1>Заполните форму, чтобы сформировать резюме</h1>
        <div class="row my-3">
            <div class="col">
                <h3>Профессия:</h3>
                <input type="text" class="form-control" placeholder="Профессия" v-model="profession">
            </div>
            <div class="col">
                <h3>Город:</h3>
                <input type="text" class="form-control" placeholder="Город" v-model="city">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h3>Ссылка на фото:</h3>
                <input type="text" class="form-control" placeholder="Ссылка на фото" v-model="image">
            </div>
            <div class="col">
                <h3>ФИО:</h3>
                <input type="text" class="form-control" placeholder="ФИО" v-model="FIO">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h3>Телефон:</h3>
                <input type="number" class="form-control" placeholder="Телефон" v-model="phoneNumber">
            </div>
            <div class="col">
                <h3>E-mail:</h3>
                <input type="text" class="form-control" placeholder="E-mail" v-model="email">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h3>Дата рождения:</h3>
                <input type="date" class="form-control" placeholder="Дата рождения" v-model="birthdate">
            </div>
            <div class="col">
                <h3>Образование:</h3>
                <input type="text" class="form-control" placeholder="Образование" v-model="educationLevel">
            </div>
        </div>
        <div class="row my-3">
            <div class="col">
                <h3>Желаемая зарплата:</h3>
                <input type="number" class="form-control" placeholder="Желаемая зарплата" v-model="salary">
            </div>
            <div class="col">
                <h3>Ключевые навыки:</h3>
                <input type="text" class="form-control" placeholder="Ключевые навыки" v-model="keySkills">
            </div>
        </div>
        <div class="row">
            <h3>О себе:</h3>
            <textarea rows="5" placeholder="О себе" v-model="about"></textarea>
        </div>
        <button type="button" class="btn btn-success my-3">Создать резюме</button>
    </div>
    <div class="container">
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
                <h4>Желаемая зарплата: {{ salary }}</h4>
                <h4>Ключевые навыки: {{ keySkills }}</h4>
                <h4>О себе: {{ about }}</h4>
            </div>
        </div>
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
            salary: null,
            keySkills: null,
            about: null,
        }
    }
}
</script>