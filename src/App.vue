<template>
  <UploadWidget />
  <img width="400" id="uploadedimage" src="" />
</template>

<script>
import UploadWidget from "./components/UploadWidget.vue";
export default {
  name: "App",
  components: {
    UploadWidget: UploadWidget,
  },
};
</script>

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
