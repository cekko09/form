<template>
  <div>
    <div class="container_fluid">

      <transition name="fade" mode="out-in">
        <div v-if="!settings_done" class="settings_container">
          <div class="settings">
            <form @submit.prevent="saveSettings" class="m-auto">
              <div class="form-group">
                <label class="form-label" for="primaryColor">Primary Color</label>
                <input class="form-control" type="color" v-model="primaryColor" id="primaryColor" :disabled="useDefaultPrimaryColor" />
                <input  type="checkbox" v-model="useDefaultPrimaryColor" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="secondaryColor">Secondary Color</label>
                <input class="form-control" type="color" v-model="secondaryColor" id="secondaryColor" :disabled="useDefaultSecondaryColor" />
                <input  type="checkbox" v-model="useDefaultSecondaryColor" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="tertiaryColor">Tertiary Color</label>
                <input class="form-control" type="color" v-model="tertiaryColor" id="tertiaryColor" :disabled="useDefaultTertiaryColor" />
                <input  type="checkbox" v-model="useDefaultTertiaryColor" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="formBackgroundImage">Form Background Image</label>
                <input class="form-control" type="text" v-model="formBackgroundImage" id="formBackgroundImage"
                  :disabled="useDefaultFormBackgroundImage" />
                <input  type="checkbox" v-model="useDefaultFormBackgroundImage" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="pageBackgroundImage">Page Background Image</label>
                <input class="form-control" type="text" v-model="pageBackgroundImage" id="pageBackgroundImage"
                  :disabled="useDefaultPageBackgroundImage" />
                <input  type="checkbox" v-model="useDefaultPageBackgroundImage" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="pageTitle">Page Title</label>
                <input class="form-control" type="text" v-model="pageTitle" id="pageTitle" />
              </div>

              <div class="form-group">
                <label class="form-label" for="pageDesc">Page Description</label>
                <input class="form-control" type="text" v-model="pageDesc" id="pageDesc" />
              </div>

              <div class="form-group">
                <label class="form-label" for="logo">Logo</label>
                <input class="form-control" type="text" v-model="logo" id="logo" :disabled="useDefaultLogo" />
                <input  type="checkbox" v-model="useDefaultLogo" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="redirectUrl">Redirect Url</label>
                <input class="form-control" type="text" v-model="redirectUrl" id="redirectUrl" :disabled="useDefaultRedirectUrl" />
                <input  type="checkbox" v-model="useDefaultRedirectUrl" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="welcomeMessage">Welcome Message Title</label>
                <input class="form-control" type="text" v-model="welcomeMessage.title" id="welcomeMessage"
                  :disabled="useDefaultWelcomeMessageTitle" />
                <input  type="checkbox" v-model="useDefaultWelcomeMessageTitle" /> Use Default
              </div>
              <div class="form-group">
                <label class="form-label" for="welcomeMessage">Welcome Message Description</label>
                <input class="form-control" type="text" v-model="welcomeMessage.description" id="welcomeMessage"
                  :disabled="useDefaultWelcomeMessageDesc" />
                <input  type="checkbox" v-model="useDefaultWelcomeMessageDesc" /> Use Default
              </div>

              <div class="form-group">
                <label class="form-label" for="successMessage">Success Message</label>
                <input class="form-control" type="text" v-model="successMessage" id="successMessage" :disabled="useDefaultSuccessMessage" />
                <input  type="checkbox" v-model="useDefaultSuccessMessage" /> Use Default
              </div>

              <button type="submit">Submit</button>
            </form>
          </div>

        </div>
        <component v-else :is="'DynamicForm'" :key="'dynamicForm'" :pageBackgroundImage="pageBackgroundImage"
          :formBackgroundImage="formBackgroundImage" :tertiaryColor="tertiaryColor" :pageTitle="pageTitle"
          :pageDesc="pageDesc" :secondaryColor="secondaryColor" :primaryColor="primaryColor" :logo="logo"
          :loading="loading" :redirectUrl="redirectUrl" :welcomeMessage="welcomeMessage"
          :successMessage="successMessage" v-bind="!showWelcome ? dynamicFormProps : {}" />
      </transition>
    </div>

  </div>

</template>

<script>

export default {
  data() {
    return {
      showWelcome: true,
      welcomeMessage: {
        title: '',
        description: ''
      },
      loading: true,
      pageTitle: '',
      pageDesc: '',
      logo: null,
      primaryColor: null,
      secondaryColor: null,
      tertiaryColor: null,
      redirectUrl: null,
      successMessage: '',
      formBackgroundImage: null,
      pageBackgroundImage: null,
      settings_done: false,

      settings: null,
      useDefaultPrimaryColor: false,
      useDefaultSecondaryColor: false,
      useDefaultTertiaryColor: false,
      useDefaultFormBackgroundImage: false,
      useDefaultPageBackgroundImage: false,
      useDefaultLogo: false,
      useDefaultRedirectUrl: false,
      useDefaultWelcomeMessageTitle: false,
      useDefaultWelcomeMessageDesc: false,
      useDefaultSuccessMessage: false,

      // Store previous custom values
      previousPrimaryColor: null,
      previousSecondaryColor: null,
      previousTertiaryColor: null,
      previousFormBackgroundImage: null,
      previousPageBackgroundImage: null,
      previousLogo: null,
      previousRedirectUrl: null,
      previousWelcomeMessageTitle: null,
      previousWelcomeMessageDescription: null,
      previousSuccessMessage: null,
    };
  },

  async created() {
    try {
      const settingsResponse = await fetch('/form_settings.json');
      if (!settingsResponse.ok) {
        throw new Error('Settings data network response was not ok');
      }
      const settings = await settingsResponse.json();
      this.settings = settings; // Settings verisini data içinde sakla

      // Değerleri yükle
      this.logo = settings.logo || settings.default_settings.company_form_settings_logo;
      this.redirectUrl = settings.redirect_url || settings.default_settings.company_form_settings_redirect_url;
      this.primaryColor = settings.form_colors.primary_color || settings.default_settings.company_form_settings_primary_color;
      this.secondaryColor = settings.form_colors.secondary_color || settings.default_settings.company_form_settings_secondary_color;
      this.tertiaryColor = settings.form_colors.tertiary_color || settings.default_settings.company_form_settings_tertiary_color;
      this.welcomeMessage = {
        title: settings.options.welcome_message.title || settings.default_settings.company_form_settings_welcome_message.title,
        description: settings.options.welcome_message.description || settings.default_settings.company_form_settings_welcome_message.description
      };
      this.pageTitle = settings.options.page.page_title;
      this.pageDesc = settings.options.page.page_description;
      this.successMessage = settings.success_message || settings.default_settings.company_form_settings_success_message;
      this.formBackgroundImage = settings.form_background_image || settings.default_settings.company_form_settings_form_background_image;
      this.pageBackgroundImage = settings.page_background_image || settings.default_settings.company_form_settings_page_background_image;

      this.loading = false;

      this.applyDefaultValues();
    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }
  },

  watch: {
    // Varsayılan check için izleyiciler
    primaryColor(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_primary_color) {
        this.useDefaultPrimaryColor = true;
      } else {
        this.useDefaultPrimaryColor = false;
      }
    },
    secondaryColor(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_secondary_color) {
        this.useDefaultSecondaryColor = true;
      } else {
        this.useDefaultSecondaryColor = false;
      }
    },
    tertiaryColor(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_tertiary_color) {
        this.useDefaultTertiaryColor = true;
      } else {
        this.useDefaultTertiaryColor = false;
      }
    },
    formBackgroundImage(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_form_background_image) {
        this.useDefaultFormBackgroundImage = true;
      } else {
        this.useDefaultFormBackgroundImage = false;
      }
    },
    pageBackgroundImage(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_page_background_image) {
        this.useDefaultPageBackgroundImage = true;
      } else {
        this.useDefaultPageBackgroundImage = false;
      }
    },
    logo(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_logo) {
        this.useDefaultLogo = true;
      } else {
        this.useDefaultLogo = false;
      }
    },
    redirectUrl(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_redirect_url) {
        this.useDefaultRedirectUrl = true;
      } else {
        this.useDefaultRedirectUrl = false;
      }
    },
    'welcomeMessage.title'(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_welcome_message.title) {
        this.useDefaultWelcomeMessageTitle = true;
      } else {
        this.useDefaultWelcomeMessageTitle = false;
      }
    },
    'welcomeMessage.description'(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_welcome_message.description) {
        this.useDefaultWelcomeMessageDesc = true;
      } else {
        this.useDefaultWelcomeMessageDesc = false;
      }
    },
    successMessage(newVal) {
      if (this.settings && newVal === this.settings.default_settings.company_form_settings_success_message) {
        this.useDefaultSuccessMessage = true;
      } else {
        this.useDefaultSuccessMessage = false;
      }
    }
  },

  methods: {
    saveSettings() {
      this.settings_done = true;
    },

    applyDefaultValues() {
      if (this.settings) {
        if (this.useDefaultPrimaryColor) this.primaryColor = this.settings.default_settings.company_form_settings_primary_color;
        if (this.useDefaultSecondaryColor) this.secondaryColor = this.settings.default_settings.company_form_settings_secondary_color;
        if (this.useDefaultTertiaryColor) this.tertiaryColor = this.settings.default_settings.company_form_settings_tertiary_color;
        if (this.useDefaultFormBackgroundImage) this.formBackgroundImage = this.settings.default_settings.company_form_settings_form_background_image;
        if (this.useDefaultPageBackgroundImage) this.pageBackgroundImage = this.settings.default_settings.company_form_settings_page_background_image;
        if (this.useDefaultLogo) this.logo = this.settings.default_settings.company_form_settings_logo;
        if (this.useDefaultRedirectUrl) this.redirectUrl = this.settings.default_settings.company_form_settings_redirect_url;
        if (this.useDefaultWelcomeMessageTitle) {
          this.welcomeMessage.title = this.settings.default_settings.company_form_settings_welcome_message.title;
        }
        if (this.useDefaultWelcomeMessageDesc) {
          this.welcomeMessage.description = this.settings.default_settings.company_form_settings_welcome_message.description;
        }
        if (this.useDefaultSuccessMessage) this.successMessage = this.settings.default_settings.company_form_settings_success_message;
      }
    },
  }
};
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

h1,
p {
  text-align: center;
  margin-top: 20px;
}

.container_fluid {
  margin: 0 auto;
}

.settings_container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

.settings {
  background-color: #ffffff;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 100%;
}


.form-group {
  margin-bottom: 20px;
}

.form-label {
  font-weight: 600;
  margin-bottom: 8px;
  display: block;
  font-size: 14px;
  color: #333333;
}

.form-control {
  width: 100%;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 5px;
  font-size: 14px;
}

input[type="checkbox"] {
  margin-left: 10px;
  transform: scale(1.2);
}

button[type="submit"] {
  background-color: #007bff;
  color: #ffffff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  width: 100%;
  transition: background-color 0.3s ease;
}

button[type="submit"]:hover {
  background-color: #0056b3;
}


.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to  {
  opacity: 0;
}

@media (max-width: 768px) {
  .settings {
    padding: 20px;
  }

  .form-label {
    font-size: 12px;
  }

  .form-control {
    font-size: 12px;
  }

  button[type="submit"] {
    font-size: 14px;
  }
}

</style>
