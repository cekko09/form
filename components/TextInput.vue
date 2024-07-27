<template>
    <div class="form-group">
      <label :for="field.form_field_type_id">{{ field.label }}</label>
      <input
        :type="field.form_field_type.type"
        v-model="value"
        :id="field.form_field_type_id"
        :name="field.form_field_type_id"
        :placeholder="field.placeholder || ''"
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
        type: [String, Number],
        default: '',
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
    width: 100%;
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
  