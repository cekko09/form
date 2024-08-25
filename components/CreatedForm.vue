<template>
  <div class="form_container">

    <transition name="fade" mode="out-in">

    
    <div >
      <div class="logo_container text-center">
        <h3 :style="{ color: secondaryColor }">{{ welcomeMessage.description }}</h3>
        <img :src="logo" class="img-fluid" height="300" width="300" alt="">
      </div>
      <form onkeydown="return event.key != 'Enter';" @submit.prevent="handleFinalSubmit">
        <transition name="fade" mode="out-in">
          <div v-if="loading" :key="'loading'">
            <p>Yükleniyor...</p>
          </div>
          <div v-else :key="currentStep">
            <div v-if="currentStep < formFieldData.length">
              <component :is="getComponent(currentField)" :field="currentField" v-if="!currentField.is_hidden"
                v-model="formDataLocal[currentField.unique_id]" />
              <div class="button-container">
                <button type="button" :style="{ backgroundColor: primaryColor, borderColor: primaryColor }" class="btn"
                  @click="prevStep" v-if="currentStep > 0">Geri</button>
                <button type="button" :style="{ backgroundColor: primaryColor, borderColor: primaryColor }" class="btn"
                  @click="nextStep" v-show="currentStep < formFieldData.length - 1">İleri</button>
                <button type="submit" :style="{ backgroundColor: primaryColor, borderColor: primaryColor }" class="btn"
                  v-show="currentStep === formFieldData.length - 1">Gönder</button>
              </div>
            </div>
            <div v-else>
              <h1 :style="{ color: secondaryColor }">{{ successMessageData }}</h1>
              <p :style="{ color: secondaryColor }">{{ timer }} Saniye içinde yönlendirileceksiniz</p>
            </div>
          </div>
        </transition>
      </form>
    </div>
  </transition>

    <div class="page_details">
      <h5 :style="{ color: tertiaryColor }"> {{ pageTitle }}</h5>
      <p :style="{ color: tertiaryColor }"> {{ pageDesc }}</p>
    </div>

  </div>

</template>

<script>

export default {
props: {
  formFields: {
    type: Array,
    required: true
  },
  formData: {
    type: Object,
    required: true
  },
  pageBackgroundImage: {
    type: String,
    required: true
  },
  tertiaryColor: {
    type: String,
    required: true
  },
  pageTitle: {
    type: String,
    required: true
  },
  pageDesc: {
    type: String,
    required: true
  },
  secondaryColor: {
    type: String,
    required: true
  },
  primaryColor: {
    type: String,
    required: true
  },
  logo: {
    type: String,
    required: true
  },
  welcomeMessage: {
    type: Object,
    required: true
  },
  redirectUrl: {
    type: String,
    required: true
  },
  successMessage: {
    type: String,
    required: true
  },

  loading: {
    type: Boolean,
    required: true
  }

},

  data() {
    return {
      currentStep: 0,
      formFieldData: this.formFields,
      formDataLocal: this.formData,
      timer: 5,
      showWelcome: true,
      
      successMessageData: this.successMessage,

    };
  },


mounted() {
  console.log(this.currentField);


},

  computed: {
    currentField() {
      return this.formFieldData[this.currentStep];
    },
  },
  watch: {
    currentStep(newVal) {
      if (newVal >= this.formFieldData.length) {
        this.startCountdown();
        this.finalLog();
      }
    },
  },
  methods: {
    startForm() {
      this.showWelcome = false;
    },
    finalLog() {
      console.log(this.formDataLocal);

    },
    startCountdown() {
      this.interval = setInterval(() => {
        if (this.timer > 0) {
          this.timer--;
        } else {
          clearInterval(this.interval);
          this.redirectToThankYouPage();
        }
      }, 1000); 
    },
    redirectToThankYouPage() {
      window.location.href = this.redirectUrl;
    },
    getComponent(field) {
      switch (field.form_field_type.type) {
    case 'transaction_type':
    case 'property_type':
    case 'salutation':
    case 'selectbox':
      return 'SelectBox';
    case 'checkbox':
      return 'CheckBox';
    case 'yes_no':
    case 'radio':
      return 'RadioButton';
    case 'textarea':
      return 'TextArea';
    case 'rating':
      return 'Rating';
    case 'firstname':
    case 'surname':
    case 'address':
    case 'number':
    case 'date':
    case 'email':
    case 'phone':
    return 'TextInput';
  
    default:
      return 'TextInput';
  }
    },
    nextStep() {
      const currentField = this.formFieldData[this.currentStep];

      if (currentField.is_required && currentField.form_field_options.option_value != '' && this.formData[currentField.unique_id] == '' || currentField.is_required &&  !this.formData[currentField.unique_id] && currentField.form_field_options.option_value != '' ) {
        this.$swal.fire({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        text: 'Please fill the required space',
        timer: 3000,
        icon: "error",
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.onmouseenter = Swal.stopTimer;
          toast.onmouseleave = Swal.resumeTimer;
        }
      });
      } else {
        this.errorMessage = '';
        if (this.currentStep < this.formFieldData.length - 1) {
          this.currentStep++;
        }
      }
    },
    prevStep() {
      if (this.currentStep > 0) {
        this.currentStep--;
      }
    },
    handleFinalSubmit() {
      const currentField = this.formFieldData[this.currentStep];

      if (currentField.is_required && currentField.form_field_options.option_value != '' && this.formData[currentField.unique_id] == '' || currentField.is_required &&  !this.formData[currentField.unique_id] && currentField.form_field_options.option_value != '' ) {
        this.$swal.fire({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        text: 'Please fill the required space',
        timer: 3000,
        icon: "error",
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.onmouseenter = Swal.stopTimer;
          toast.onmouseleave = Swal.resumeTimer;
        }
      });
      } else {
        this.errorMessage = '';
        this.currentStep++;
      }
    },
  },
};
</script>

<style scoped>
.page_details {
position: absolute;
bottom: 0;
left: 0;
padding: 20px;
}
h3 {
  font-size: 2rem;
  font-weight: bold;
  color: #A18770;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.form_container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  max-width: 600px;
  margin: auto;

}

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
