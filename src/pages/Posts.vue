<template>
  <q-page padding class="q-mx-xl">
    <!-- <pre>{{ posts }}</pre> -->
    <h3>
      {{ estado }}
    </h3>
    <h4>Posts</h4>
    <ul>
      <li v-for="(item, key) in posts" :key="key">
        {{ key }} - {{ item.title }}
        <router-link :to="`posts/${key}`">Detalle</router-link>
        <q-btn color="red" @click="handleDelete(key)">Eliminar</q-btn>
      </li>
    </ul>
  </q-page>
</template>

<script>
import axios from "axios";
import Vue from "vue";
export default {
  data() {
    return {
      posts: {},
      estado: "<p>hola</p>",
    };
  },
  methods: {
    handleDelete(key) {
      console.log(key);

      //   delete this.posts[key];
      //   console.log(this.posts);

      axios
        .delete(
          `https://api-rest-cc493-default-rtdb.firebaseio.com/posts/${key}.json`
        )
        .then((res) => {
          console.log(res);
          Vue.delete(this.posts, key);
        })
        .catch((e) => console.log(e));
    },
  },
  created() {
    axios
      .get("https://api-rest-cc493-default-rtdb.firebaseio.com/posts.json")
      .then((res) => {
        // console.log(res.data);
        this.posts = res.data;
      })
      .catch((e) => console.log(e));
  },
};
</script>
