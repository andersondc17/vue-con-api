<script setup>
import { ref } from "@vue/reactivity";
import ButtonCounter from "./components/ButtonCounter.vue";
import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import { computed, onMounted } from "@vue/runtime-core";
import LoadingSpinner from "./components/LoadingSpinner.vue";

const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref("");
const cambiarFavorito = (post) => {
  favorito.value = post;
};

const next = () => {
  inicio.value = inicio.value + postXpage;
  fin.value = fin.value + postXpage;
};

const prev = () => {
  inicio.value -= postXpage;
  fin.value -= postXpage;
};

/*
onMounted(async () => {
  //loading.value = true;
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log(error);
  } finally {
    setTimeout(() => {
      loading.value = false;
    }, 2000);
  }
});
*/

/*
fetch("https://jsonplaceholder.typicode.com/posts")
  .then((res) => res.json())
  .then((data) => {
    posts.value = data;
  })
  .catch(e => console.log(e))
  .finally(()=> {
    setTimeout(()=>{
      loading.value = false
    }, 2000)
  })
*/

const fetchData = async() => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts");
    posts.value = await res.json();
  } catch (error) {
    console.log(error);
  } finally {
    setTimeout(() => {
      loading.value = false;
    }, 2000);
  }
}

fetchData()

const maxLength = computed(() => posts.value.length);
</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h1>App</h1>
    <h2>Mis pots Favoritos: {{ favorito }}</h2>

    <PaginatePost
      @next="next"
      @prev="prev"
      class="mb-2"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength"
    />

    <!--
    <ButtonCounter/>
    <button-counter/>

    <BlogPost title="Post 1" :id="1" body="descripcion 1"/>
    <BlogPost title="Post 2" :id="2" body="descripcion 2"/>
    <BlogPost title="Post 3" :id="3" body="descripcion 3"/>
    <BlogPost title="Post 4" :id="4"/>
    -->

    <BlogPost
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title"
      :id="post.id"
      :body="post.body"
      :cambiarFavorito="cambiarFavorito"
      class="mb-2"
    >
    </BlogPost>
  </div>
</template> 