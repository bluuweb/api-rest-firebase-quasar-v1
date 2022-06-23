<template>
  <q-page padding class="q-mx-xl">
    <form @submit.prevent="handleSubmit">
      <q-input v-model="title" label="Titulo"></q-input>
      <q-input v-model="description" label="Descripción"></q-input>
      <q-btn type="submit">Enviar</q-btn>
    </form>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  name: "PageIndex",
  data() {
    return {
      title: "",
      description: "",
    };
  },
  methods: {
    handleSubmit() {
      console.log("todo ok");
      axios
        .post("https://api-rest-cc493-default-rtdb.firebaseio.com/posts.json", {
          title: this.title,
          description: this.description,
          date: Date.now(),
          author: "111",
          comments: [],
        })
        .then((response) => {
          console.log(response.data);
        })
        .catch((e) => console.log(e));
    },
  },
};
</script>
