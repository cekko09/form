<template>
  <div>
    <form class="m-auto" @submit.prevent="handleFinalSubmit">
      <div v-if="currentStep < formFields.length">
        <component
          :is="getComponent(currentField)"
          :field="currentField"
          v-model="formData[currentField.unique_id]"
        />
        <div class="button-container">
          <button :style="{ backgroundColor: primaryColor, borderColor: primaryColor }"  type="button" class="btn" @click="prevStep" v-if="currentStep > 0">Geri</button>
          <button :style="{ backgroundColor: primaryColor, borderColor: primaryColor }"  type="button" class="btn" @click="nextStep" v-if="currentStep < formFields.length - 1">İleri</button>
          <button :style="{ backgroundColor: primaryColor, borderColor: primaryColor }"  type="submit" class="btn" v-if="currentStep === formFields.length - 1">Gönder</button>
        </div>
      </div>
      <div v-else>
        <p>Formunuz gönderildi!</p>
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
  async created() {
    try {
      const settingsResponse = await fetch('/form_settings.json');
      if (!settingsResponse.ok) {
        throw new Error('Settings data network response was not ok');
      }
      const settings = await settingsResponse.json();

      this.primaryColor = settings.form_colors.primary_color || settings.default_settings.company_form_settings_primary_color;
      this.secondaryColor = settings.form_colors.secondary_color || settings.default_settings.company_form_settings_secondary_color;
      this.tertiaryColor = settings.form_colors.tertiary_color || settings.default_settings.company_form_settings_tertiary_color;
     
  

    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }
  },

  computed: {
    currentField() {
      return this.formFields[this.currentStep];
    },
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
    nextStep() {
      if (this.currentStep < this.formFields.length - 1) {
        this.currentStep++;
      }
    },
    prevStep() {
      if (this.currentStep > 0) {
        this.currentStep--;
      }
    },
    handleFinalSubmit() {
      console.log(this.formData);
      this.currentStep++;
    },
  },
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
