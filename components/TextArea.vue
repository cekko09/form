<template>
    <div class="form-group">
      <label :for="field.unique_id">{{ field.label }}</label>
      <textarea
        :id="field.unique_id"
        :name="field.unique_id"
        :value="inputValue"
    @input="$emit('input', $event.target.value)"
        
       :placeholder="field.placeholder || ''"
        class="form-control"
      > </textarea>
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
        type: String,
        default: '',
      },
    },
    data() {
      return {
        inputValue: this.value,
      };
    }
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
  