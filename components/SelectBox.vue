<template>
    <div class="">
      <label>{{ field.label }}</label>
      <select
        v-model="value"
        :id="field.unique_id"
        :name="field.unique_id"
        :required="field.is_required"
        class="form-control"
      >
      <option
      value="" disabled selected hidden
        >
  Select Your Option        </option>
        <option
          v-for="(option, index) in field.form_field_options"
          :key="index"
          :value="option.option_value"
        >
          {{ option.option_label }}
        </option>
      </select>
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
        type: [String, Number],
        default: '',
      },
    },
  };
  </script>
  
  <style scoped>
 
  </style>
  