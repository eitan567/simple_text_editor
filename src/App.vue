<template>
  <TextEditor value="test text editor" dir="rtl" align="right" />
</template>

<script>
import TextEditor from "./components/TextEditor.vue";

export default {
  name: "App",
  components: {
    TextEditor,
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
