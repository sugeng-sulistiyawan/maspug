<template>
  <div class="text-center">
    <h1 class="h1 fw-bolder">{{ title }}</h1>
    <p class="m-0 text-muted">{{ description }}</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    title: String,
    description: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
