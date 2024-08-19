<template>
  <div>
    <form class="m-auto" onkeydown="return event.key != 'Enter';" @submit.prevent="handleFinalSubmit">
      <div v-if="currentStep < formFields.length ">
        <component
          :is="getComponent(currentField)"
          :field="currentField"
          v-if="!currentField.is_hidden"
          v-model="formData[currentField.unique_id]"
        />
        <div class="button-container">
          <button type="button" class="btn" @click="prevStep" v-if="currentStep > 0">Geri</button>
          <button type="button" class="btn" @click="nextStep" v-show="currentStep < formFields.length - 1">İleri</button>
          <button type="submit" class="btn" v-show="currentStep === formFields.length - 1" >Gönder</button>
        </div>
      </div>
      <div v-else>
          <li v-for="(value, key) in formData" :key="key">
        {{ key }}: {{ value }}
      </li>
      </div>
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
      required: true,
    },
    formData: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      currentStep: 0,
    };
  },
  computed: {
    currentField() {
      return this.formFields[this.currentStep];
    },
  },
  methods: {
    getComponent(field) {
      switch (field.form_field_type.type) {
      
        case 'selectbox':
        case 'salutation':
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
    nextStep() {
      const currentField = this.formFields[this.currentStep];
      
      if (currentField.is_required && currentField.form_field_options.option_value != '' && this.formData[currentField.unique_id] == '' || currentField.is_required &&  !this.formData[currentField.unique_id] && currentField.form_field_options.option_value != '' ){
        this.errorMessage = `${currentField.label} alanı doldurulmalıdır.`;
        alert(this.errorMessage);
      } else {
        this.errorMessage = '';
        if (this.currentStep < this.formFields.length - 1) {
          this.currentStep++;
        }
      }
    },
    GoToGoogle() {
      setTimeout(() => {
        window.location.href = 'https://google.com';
      }, 2000);
    },
    prevStep() {
      if (this.currentStep > 0) {
        this.currentStep--;
      }
    },
    handleFinalSubmit() {
      const currentField = this.formFields[this.currentStep];
      
      if (currentField.is_required && currentField.form_field_options.option_value != '' && this.formData[currentField.unique_id] == '' || currentField.is_required &&  !this.formData[currentField.unique_id] && currentField.form_field_options.option_value != '' ){
        this.errorMessage = `${currentField.label} alanı doldurulmalıdır.`;
        alert(this.errorMessage);
      } else {
        this.errorMessage = '';
          this.currentStep++;
      }
    },
  },
  created() {
    this.formFields.forEach(field => {
      // Varsayılan değeri formData'ya ekle
      this.$set(this.formData, field.unique_id, field.default_value || 
        (field.form_field_type.type === 'checkbox' ? [] : ''));
    });
  }
};
</script>

<style scoped>
form {
  max-width: 600px;
}
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
.button-container {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}
</style>
