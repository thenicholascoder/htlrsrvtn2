<script setup></script>

<template>
  <div class="home">
    <div class="home_container home_jumbotron center jumbotron">
      <h1 class="home_title">Welcome to the Sample App</h1>
      <h2 class="home_subtitle">
        This is the home page for the
        <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a>
        sample application.
      </h2>
      <router-link to="#" class="btn btn-lg button_home"
        >Sign up now!</router-link
      >
    </div>
    <img class="home_rails" src="@/assets/images/rails.svg" alt="Rails logo" />
    <img class="home_kitten" src="@/assets/images/kitten.png" alt="Kitten" />
  </div>
</template>

<style scoped></style>
