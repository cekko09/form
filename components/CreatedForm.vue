<template>
    <div>
      <form @submit.prevent="handleFinalSubmit">
        <div v-for="(field, index) in formFields" :key="index" class="form-field-container">
          <component
            :is="getComponent(field)"
            :field="field"
            v-model="formData[field.unique_id]"
          />
        </div>
        <button type="submit" class="btn">Gönder</button>
      </form>
    </div>
  </template>
  
  <script>
  import TextInput from './TextInput.vue';
  import SelectBox from './SelectBox.vue';
  import CheckBox from './CheckBox.vue';
  import RadioButton from './RadioButton.vue';
  import TextArea from './TextArea.vue';
  import Rating from './Rating.vue';
  
  export default {
    components: {
      TextInput,
      SelectBox,
      CheckBox,
      RadioButton,
      TextArea,
      Rating,
    },
    props: {
      formFields: {
        type: Array,
        required: true
      },
      formData: {
        type: Object,
        required: true
      }
    },
    methods: {
      getComponent(field) {
        switch (field.form_field_type.type) {
          case 'email':
          case 'number':
          case 'date':
            return 'TextInput';
          case 'selectbox':
            return 'SelectBox';
          case 'checkbox':
            return 'CheckBox';
          case 'radio':
            return 'RadioButton';
          case 'textarea':
            return 'TextArea';
          case 'rating':
            return 'Rating';
          default:
            return 'TextInput';
        }
      },
      handleFinalSubmit() {
        console.log(this.formData); 
      }
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
  .btn {
    display: inline-block;
    font-weight: 400;
    color: #212529;
    text-align: center;
    vertical-align: middle;
    cursor: pointer;
    background-color: #007bff;
    border: 1px solid #007bff;
    padding: 10px 20px;
    font-size: 16px;
    line-height: 1.5;
    border-radius: 4px;
    color: white;
    text-decoration: none;
    margin-top: 10px;
  }
  .form-field-container {
    margin-bottom: 10px;
  }
  </style>
  