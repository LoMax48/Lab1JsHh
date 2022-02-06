<template>
  <div class="container-fluid">
    <h1 class="display-4">Главная страница</h1>
    <div v-if="loading" class="spinner-border text-primary" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
    <div v-if="loaded" class=" row ">
      <div class="row d-flex justify-content-between">
        <div class="col-3">
          <h3>Новые</h3>
          <draggable
            class="list-group new "
            :list="newResumes"
            group="people"
            @end="log"
            itemKey="full_name"
          >
            <template #item="{ element }">
              <div class="list-group-item m-2">
                <img
                  v-bind:src="element.url_photo"
                  class="card-img-top"
                  alt="..."
                  style="height: 8rem; width: 10rem"
                />
                {{ element.full_name }}
                {{ element.profession }}
                {{ element.date_of_birth }}
                <router-link
                  v-bind:to="'/edit/' + element.id"
                  class="btn btn-primary fs-6"
                  >Открыть</router-link
                >
              </div>
            </template>
          </draggable>
        </div>

        <div class="col-3">
          <h3>Назначено собеседование</h3>
          <draggable
            class="list-group scheduledInterviewResumes"
            :list="scheduledInterviewResumes"
            group="people"
            @end="log"
            itemKey="full_name"
          >
            <template #item="{ element }">
              <div class="list-group-item m-2">
                <img
                  v-bind:src="element.url_photo"
                  class="card-img-top"
                  alt="..."
                  style="height: 8rem; width: 10rem"
                />
                {{ element.full_name }}
                {{ element.profession }}
                {{ element.date_of_birth }}
                <router-link
                  v-bind:to="'/edit/' + element.id"
                  class="btn btn-primary fs-6"
                  >Открыть</router-link
                >
              </div>
            </template>
          </draggable>
        </div>
        <div class="col-3">
          <h3>Принятые</h3>
          <draggable
            class="list-group acceptedResumes"
            :list="acceptedResumes"
            group="people"
            @end="log"
            itemKey="full_name"
          >
            <template #item="{ element }">
              <div class="list-group-item m-2">
                <img
                  v-bind:src="element.url_photo"
                  class="card-img-top"
                  alt="..."
                  style="height: 8rem; width: 10rem"
                />
                {{ element.full_name }}
                {{ element.profession }}
                {{ element.date_of_birth }}
                <router-link
                  v-bind:to="'/edit/' + element.id"
                  class="btn btn-primary fs-6"
                  >Открыть</router-link
                >
              </div>
            </template>
          </draggable>
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
import axios from "axios";
import draggable from "vuedraggable";
export default {
  name: "HomePageComponent",
  data() {
    return {
      loading: true,
      loaded: false,
      newResumes: [],
      scheduledInterviewResumes: [],
      acceptedResumes: [],
    };
  },
  components: {
    draggable,
  },
  watch: {
    $route: "getData",
  },
  methods: {
    log: function (evt) {
      console.log(evt.to.classList[1]);
      if (evt.to.classList[1] == "new") {
        axios
          .put(
            "http://localhost:8080/api/cv/" +
              evt.item._underlying_vm_.id +
              "/status/edit",
            {
              id: evt.item._underlying_vm_.id,
              status_of_resume: "Новый",
            }
          )
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
      if (evt.to.classList[1] == "scheduledInterviewResumes") {
        axios
          .put(
            "http://localhost:8080/api/cv/" +
              evt.item._underlying_vm_.id +
              "/status/edit",
            {
              id: evt.item._underlying_vm_.id,
              status_of_resume: "Назначено собеседование",
            }
          )
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
      if (evt.to.classList[1] == "acceptedResumes") {
        axios
          .put(
            "http://localhost:8080/api/cv/" +
              evt.item._underlying_vm_.id +
              "/status/edit",
            {
              id: evt.item._underlying_vm_.id,
              status_of_resume: "Принят",
            }
          )
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:8080/api/cv")
      .then((response) => {
        console.log(response);
        response.data.forEach((resumeItem) => {
          if (resumeItem.status_of_resume == "Новый") {
            this.newResumes.push(resumeItem);
          }
          if (resumeItem.status_of_resume == "Назначено собеседование") {
            this.scheduledInterviewResumes.push(resumeItem);
          }
          if (resumeItem.status_of_resume == "Принят") {
            this.acceptedResumes.push(resumeItem);
          }
        });
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => ((this.loading = false), (this.loaded = true)));
  },
};
</script>
