<template>
  <div>
    <h1>Dinamik Form Oluşturucu</h1>
    <DynamicForm />
  </div>
</template>

<script>
import DynamicForm from '~/components/DynamicForm.vue';

export default {
  components: {
    DynamicForm,
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

h1 {
  text-align: center;
  margin-top: 20px;
}
</style>
