<template>
    <div class="form-group">
      <label>{{ field.label }}</label>
      <div v-for="(option, index) in field" :key="index" class="form-check">
        <input
          :type="field.type"
          v-model="value"
          :id="`${field.form_field_type_id}-${index}`"
          :name="field.form_field_type_id"
          :value="option.type"
          
          class="form-check-input"
        />
        <label :for="`${field.form_field_type_id}-${index}`" class="form-check-label">
          {{ option.type }}
        </label>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      field: {
        type: Object,
        required: true,
      },
      value: {
        type: Object,
        default: '',
      },
    },
  };
  </script>
  
  <style scoped>
  .form-group {
    margin-bottom: 15px;
  }
  .form-check {
    margin-bottom: 10px;
  }
  .form-check-input {
    margin-right: 10px;
  }
  </style>
  