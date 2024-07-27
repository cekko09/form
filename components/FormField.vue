<template>
    <div class="form-group">
      <label :for="field.form_field_type_id">{{ field.label }}</label>
      <component
        :is="getComponent(field)"
        v-model="formData[field]"
        v-bind="getAttributes(field)"
        class="form-control"
      >
       
      </component>
    </div>
  </template>
  
  <script>
  export default {
    props: {
     field: {
        type: Object,
        required: true,
      },
      formData: {
        type: Object,
        required: true
      }
    },
    methods: {
  getComponent(field) {
    console.log('Field:', field);
    if (!field || !field.type) {
      return 'input';
    }
    switch (field.type) {
      case 'email':
      case 'number':
      case 'date':
        return 'input';
      case 'selectbox':
        return 'select';
      case 'checkbox':
      case 'radio':
        return 'input';
      case 'textarea':
        return 'textarea';
      default:
        return 'input';
    }
  },
  getAttributes(field) {
    if (!field || !field.type) {
      return {};
    }
    let attributes = {
      id: field.unique_id,
      name: field.unique_id,
      placeholder: field.placeholder || '',
      required: field.is_required ? true : undefined,
    };

    if (field.type === 'selectbox') {
      attributes.multiple = field.multiple || false;
    }

    if (['checkbox', 'radio', 'number', 'email', 'date'].includes(field.type)) {
      attributes.type = field.type;
    }

    return attributes;
  },
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
  