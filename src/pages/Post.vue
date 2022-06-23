<template>
  <q-page padding class="q-mx-xl">
    <!-- <pre>{{ post }}</pre> -->
    <h4>{{ post.title }}</h4>
    <p>{{ post.description }}</p>
    <form @submit.prevent="handleComment">
      <!-- <q-input v-model="text" label="Ingrese comentario"></q-input> -->
      <q-editor v-model="text" min-height="5rem" />
      <q-btn type="submit">Agregar</q-btn>
    </form>
    {{ text }}
    <ul>
      <li v-for="(item, index) in post.comments" :key="index">
        <!-- {{ item.text }} -->
        <span v-html="item.text"></span>
      </li>
    </ul>
  </q-page>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      post: {},
      text: "",
    };
  },
  methods: {
    handleComment() {
      this.post.comments.push({
        text: this.text,
        date: Date.now(),
        uid: "111",
      });

      axios
        .patch(
          `https://api-rest-cc493-default-rtdb.firebaseio.com/posts/${this.$route.params.id}.json`,
          {
            comments: this.post.comments,
          }
        )
        .then((res) => {
          console.log(res.data);
        })
        .catch((e) => console.log(e));
    },
  },
  created() {
    axios
      .get(
        `https://api-rest-cc493-default-rtdb.firebaseio.com/posts/${this.$route.params.id}.json`
      )
      .then((res) => {
        console.log(res.data);
        if (!res.data.comments) {
          this.post = res.data;
          this.post.comments = [];
        }
        this.post = res.data;
      })
      .catch((e) => console.log(e));
  },
};
</script>
