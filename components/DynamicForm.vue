<template>
  <div class="container">
    <!-- Eğer form gönderilmemişse, form elemanı seçimini göster -->
    <div v-if="!isSubmitted">
      <div class="form-group">
        <label for="formElementSelector">Form Elemanı Seçin</label>
        <select v-model="selectedField" @change="addField" class="form-control" id="formElementSelector">
          <option value="" disabled selected>Bir form elemanı seçin</option>
          <option v-for="(field, index) in availableFields" :value="field" :key="index">{{ field.form_field_type.detail }}</option>
        </select>
      </div>

      <div class="row">
        <div class="col-md-3 form_settings" v-if="selectedFormField">
          <div>
            <p>Form Elemanı Özellikleri</p>

            <div class="form-group">
              <label for="label">Label</label>
              <input type="text" v-model="selectedFormField.label" class="form-control" id="label" />
            </div>
            <div class="form-group">
              <label for="isRequired">Required</label>
              <input type="checkbox" v-model="selectedFormField.is_required" id="isRequired" />
            </div>
            <div v-if="showPlaceholder">
              <div class="form-group">
                <label for="placeholder">Placeholder</label>
                <input type="text" v-model="selectedFormField.placeholder" class="form-control" id="placeholder" />
              </div>
            </div>
            <div v-if="selectedFormField.form_field_type.type === 'selectbox' || selectedFormField.form_field_type.type === 'checkbox' || selectedFormField.form_field_type.type === 'radio'" class="form-group">
              <label for="options">Options</label>
              <div v-for="(option, index) in selectedFormField.form_field_options" :key="index" class="form-group">
                <input type="text" v-model="option.option_label" placeholder="Option Label" class="form-control" />
                <input type="text" v-model="option.option_value" placeholder="Option Value" class="form-control" />
                <button @click="removeOption(index)" class="btn btn-danger">Remove</button>
              </div>
              <button @click="addOption" class="btn btn-primary">Add Option</button>
            </div>
            <button @click="updateField" class="btn">Güncelle</button>
          </div>
        </div>
        <div class="col-md-9">
          <form @submit.prevent="handleSubmit">
            <div v-for="(field, index) in formFields" :key="index" class="form-field-container">
              <component
                :is="getComponent(field)"
                :field="field"
                v-model="formData[field.unique_id]"
                @click="selectField(field)"
              />
              <button type="button" @click="removeField(index, field)" class="btn btn-danger">Kapat</button>
              <button type="button" @click="editField(field)" class="btn btn-primary">Düzenle</button>
            </div>
            <button type="submit" class="btn">Oluştur</button>
          </form>
        </div>
      </div>
    </div>

    <!-- Eğer form gönderildiyse, CreatedForm bileşenini göster -->
    <div v-else>
      <CreatedForm :formFields="formFields" :formData="formData" />
    </div>
  </div>
</template>

<script>
import TextInput from './TextInput.vue';
import SelectBox from './SelectBox.vue';
import CheckBox from './CheckBox.vue';
import RadioButton from './RadioButton.vue';
import TextArea from './TextArea.vue';
import Rating from './Rating.vue';
import CreatedForm from './CreatedForm.vue';
export default {
  components: {
    TextInput,
    SelectBox,
    CheckBox,
    RadioButton,
    TextArea,
    Rating,
    CreatedForm, 
  },
  data() {
    return {
      selectedField: null,
      selectedFormField: null,
      availableFields: [],
      formFields: [],
      formData: {},
      isSubmitted: false, 
    };
  },
  async created() {
    try {
      const response = await fetch('/form-data.json');
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      this.availableFields = await response.json();
    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
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
    addField() {
      if (this.selectedField) {
        const newField = JSON.parse(JSON.stringify(this.selectedField));
        if (newField.form_field_type.type === 'rating') {
          this.$set(this.formData, newField.unique_id, 0);  // Ensure numeric default value
        } else {
          this.$set(this.formData, newField.unique_id, newField.form_field_type.type === 'checkbox' ? [] : '');
        }
        this.formFields.push(newField);
        this.selectedField = null;
        this.selectedFormField = newField;
      }
    },
    removeField(index, field) {
      this.formFields.splice(index, 1);
      if (this.selectedFormField && this.selectedFormField.unique_id === field.unique_id) {
        this.selectedFormField = null;
      }
    },
    editField(field) {
      this.selectedFormField = field;
    },
    updateField() {
      const index = this.formFields.findIndex(field => field.unique_id === this.selectedFormField.unique_id);
      if (index !== -1) {
        this.$set(this.formFields, index, this.selectedFormField);
      }
    },
    addOption() {
      if (this.selectedFormField && this.selectedFormField.form_field_options) {
        this.selectedFormField.form_field_options.push({ option_label: '', option_value: '' });
      }
    },
    removeOption(index) {
      if (this.selectedFormField && this.selectedFormField.form_field_options) {
        this.selectedFormField.form_field_options.splice(index, 1);
      }
    },
    handleSubmit() {
      this.isSubmitted = true;
    },
    selectField(field) {
      this.selectedFormField = field;
    },
  },
  computed: {
    showPlaceholder() {
      return this.selectedFormField && this.selectedFormField.form_field_type.type !== 'rating' && this.selectedFormField.form_field_type.type !== 'checkbox' && this.selectedFormField.form_field_type.type !== 'date' && this.selectedFormField.form_field_type.type !== 'radio';
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
.form-field-container .btn {
  margin-left: 10px;
}
.form_settings {
  border-right: 1px solid black;
}
.btn-danger {
  background-color: #dc3545;
  border-color: #dc3545;
}
</style>
