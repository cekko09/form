<template>
    <div class="form-group">
      <label :for="field.unique_id">{{ field.label }}</label>
      <input
        type="checkbox"
        v-model="value"
        :id="field.unique_id"
        :name="field.unique_id"
        :required="field.is_required"
        class="form-control"
      />
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
        type: Boolean,
        default: false,
      },
    },
  };
  </script>
  
  <style scoped>
  .form-group {
    margin-bottom: 15px;
  }
  .form-control {
    display: block;
    width: auto;
    padding: 8px;
    font-size: 14px;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 4px;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  }
  </style>
    