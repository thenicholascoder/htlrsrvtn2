<template>
  <div>
    <h1>Posts</h1>

    <input type="text"
      v-model="title"
      placeholder="Title">

    <input type="text"
      v-model="body"
      placeholder="Body">

    <!-- only render if editing posts -->
    <button 
      v-if="isEditing"
      @click="updatePost">
      Update
    </button>

    <button 
      v-if="isEditing"
      @click="cancelEdit">
      Cancel
    </button>

    <!-- only render if not editing posts -->
    <button 
      v-else
      @click="createPost"
    >
      Create
    </button>

    <!-- List of Posts -->
    <div v-for="post in posts" :key="post.id">
      <h5>[{{ post.id }}]{{ post.title }}</h5>
      <p>{{  post.body }}</p>
      <button @click="editPost(post.id)"> Edit </button>
      <button @click="deletePost(post.id)"> Delete </button>
    </div>
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue';

  const posts = ref([]);
  const title = ref('');
  const body = ref('');
  const post_id = ref(0);
  const isEditing = ref(false);
  const API_URL = "http://localhost:3000/posts";

  onMounted(async () => {
    const res = await fetch(API_URL);
    posts.value = await res.json();
  });

  const createPost = async () => {
    const res = await fetch(API_URL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        title: title.value,
        body: body.value
      })
    });

    const data = await res.json();

    posts.value.push(data);
    title.value = '';
    body.value = '';
    post_id.value = 0;
  };

  const updatePost = async () => {
    const res = await fetch(`${API_URL}/${post_id.value}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        title: title.value,
        body: body.value,
        id: post_id.value
      })
    });

    const data = await res.json();

    const index = posts.value.findIndex(post => post.id === data.id);
    posts.value[index] = data;

    title.value = '';
    body.value = '';
    post_id.value = 0;
    isEditing.value = false;
  };

  const cancelEdit = async () => {
    title.value = '';
    body.value = '';
    post_id.value = 0;
    isEditing.value = false;
  };

  const deletePost = async (id) => {
    await fetch(`${API_URL}/${id}`, {
      method: 'DELETE'
    });
    posts.value = posts.value.filter(post => post.id !== id);
  };

  const editPost = async (id) => {
    const post = posts.value.find(post => post.id === id);
    title.value = post.title;
    body.value = post.body;
    post_id.value = post.id;
    isEditing.value = true;

    window.scrollTo({
      top: 0,
      behavior: 'smooth'
    });
  };
</script>

<style scoped>
/* Add your scoped styles here */
</style>