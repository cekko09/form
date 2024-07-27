<template>
  <div>
    <!-- Form Elemanı Seçimi için Dropdown -->
    <div class="form-group">
      <label for="formElementSelector">Form Elemanı Seçin</label>
      <!-- Dropdown seçimi değiştiğinde addField metodunu tetikleyerek yeni alan ekle -->
      <select v-model="selectedField" @change="addField" class="form-control" id="formElementSelector">
        <option value="" disabled selected>Bir form elemanı seçin</option>
        <!-- Tüm mevcut form alanlarını dropdown içinde göster -->
        <option v-for="(field, index) in availableFields" :value="field" :key="index">{{ field.form_field_type.type }}</option>
      </select>
    </div>

    <!-- Form -->
    <form @submit.prevent="handleSubmit">
      <!-- formFields dizisindeki her bir alan için uygun Vue bileşenini oluştur -->
      <component
        v-for="(field, index) in formFields"
        :is="getComponent(field)"
        :key="index"
        :field="field"
        v-model="formData[field.unique_id]"
      />
      <button type="submit" class="btn">Gönder</button>
    </form>
  </div>
</template>

<script>
// Gerekli form bileşenlerini içeri aktar
import TextInput from './TextInput.vue';
import SelectBox from './SelectBox.vue';
import CheckBox from './CheckBox.vue';
import RadioButton from './RadioButton.vue';
import TextArea from './TextArea.vue';

export default {
  components: {
    TextInput,
    SelectBox,
    CheckBox,
    RadioButton,
    TextArea,
  },
  data() {
    return {
      selectedField: null, // Kullanıcının seçtiği form elemanı
      availableFields: [], // Mevcut form alanlarının listesi
      formFields: [], // Formda gösterilecek alanların listesi
      formData: {}, // Form verilerini tutar
    };
  },
  async created() {
    try {
      // form-field-types.json dosyasını fetch ile yükle
      const response = await fetch('/form-data.json');
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      // Mevcut form alanlarını availableFields değişkenine ata
      this.availableFields = await response.json();
      console.log("created");
      console.log("availableFields");
      console.log(this.availableFields);
      console.log("formFields");
      console.log(this.formFields);
      console.log("formData");
      console.log(this.formData);
    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }
  },
  methods: {
    // Alan türüne göre uygun Vue bileşenini döndür
    getComponent(field) {
      switch (field.form_field_type.type) {
        case 'email':
        case 'number':
        case 'date':
          return 'TextInput'; // TextInput bileşenini kullan
        case 'selectbox':
          return 'SelectBox'; // SelectBox bileşenini kullan
        case 'checkbox':
          return 'CheckBox'; // CheckBox bileşenini kullan
        case 'radio':
          return 'RadioButton'; // RadioButton bileşenini kullan
        case 'textarea':
          return 'TextArea'; // TextArea bileşenini kullan
        default:
          return 'TextInput'; // Varsayılan olarak TextInput bileşenini kullan
      }
    },
    // Seçilen form elemanını formFields dizisine ekle
    addField() {
      if (this.selectedField) {
        // Seçilen form elemanının kopyasını oluştur
        const newField = JSON.parse(JSON.stringify(this.selectedField));
        // formFields dizisine yeni alanı ekle
        this.formFields.push(newField);
        // formData nesnesine yeni alanı ekle ve varsayılan değeri boş string olarak ayarla
        this.$set(this.formData, newField.unique_id, '');
        // selectedField'ı null olarak ayarla, böylece dropdown sıfırlanır
        this.selectedField = null;
      console.log("formFields");
      console.log(this.formFields);
      console.log("newField");
      console.log(newField);
      console.log("formData");
      console.log(this.formData);
      }
    },
    // Form gönderildiğinde form verilerini konsola yazdır
    handleSubmit() {
      console.log(this.formData);
    },
  },
};
</script>

<style scoped>
/* Form grubu stili */
.form-group {
  margin-bottom: 15px;
}
/* Form kontrolü stili */
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
/* Buton stili */
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
</style>
