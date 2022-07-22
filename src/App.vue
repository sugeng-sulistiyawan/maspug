<template>
  <div id="app">
    <div class="container-fluid p-3 p-md-3">
      <HelloWorld title="Welcome to Your Exam" description="Cheer Up" />
      <hr />
      <FormInput />
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import FormInput from './components/FormInput.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
    FormInput,
  },
};
</script>

<style>
#app {
}
</style>
