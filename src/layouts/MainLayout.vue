<template>
  <q-layout class="bg-image">
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",
  data() {
    return {};
  }
};
</script>

<style>
.bg-image {
  background-image: url("../assets/Images/BackroundImage.jpg");

  background-repeat: no-repeat;
  background-color: black;
  background-position: center;
  background-size: cover;
}

.app-logo-image {
  background-image: url("../assets/Images/ApplicationIcon.png");

  background-repeat: no-repeat;
  background-color: black;
  background-position: center;
  background-size: cover;
}
</style>
