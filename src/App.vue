<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import { onMounted, ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'

const posts = ref([])

onMounted(() => {
  fetch('http://graph-api.test/graphql', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      query: `
        query {
          posts {
            data {
              id
              title
            }
          }
        }
      `,
    }),
  })
    .then(res => res.json())
    .then(result => {
      console.log(result)
      posts.value = result.data.posts.data
    })
})

function handleMutation() {
  fetch('http://graph-api.test/graphql', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      query: `
        mutation {
          createPostResolver(user_id: 1, title: "Hello from Vue", body: "content from vue") {
            id
            title
          }
        }
      `,
    }),
  })
    .then(res => res.json())
    .then(result => {
      console.log('Post was created')
    })
}
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3 + Vite" />
  <ul>
    <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
  </ul>
  <button @click="handleMutation">Mutation</button>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
