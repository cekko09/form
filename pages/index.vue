<template>
  <div>
    <div class="container_fluid">
     
      <transition name="fade" mode="out-in">
      <component
        :is="showWelcome ? 'Welcome' : 'DynamicForm'"
        :key="showWelcome ? 'welcome' : 'dynamicForm'"
        @startForm="startForm"
        :pageBackgroundImage="pageBackgroundImage"
        :formBackgroundImage="formBackgroundImage"
        :tertiaryColor="tertiaryColor"
        :pageTitle="pageTitle"
        :pageDesc="pageDesc"
        :secondaryColor="secondaryColor"
        :primaryColor="primaryColor"
        :logo="logo"
        :loading="loading"
        :redirectUrl="redirectUrl"
          :welcomeMessage="welcomeMessage"
          :successMessage="successMessage"
        v-bind="!showWelcome ? dynamicFormProps : {}"
      />
    </transition>
    </div>

  </div>

</template>

<script>
import DynamicForm from '~/components/DynamicForm.vue';

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
    };
  },

  async created() {
    try {
      const settingsResponse = await fetch('/form_settings.json');
      if (!settingsResponse.ok) {
        throw new Error('Settings data network response was not ok');
      }
      const settings = await settingsResponse.json();

      // Verileri component'te kullanmak üzere ayarla
      this.logo = settings.logo || settings.default_settings.company_form_settings_logo;
      this.redirectUrl = settings.redirect_url || settings.default_settings.company_form_settings_redirect_url;
      this.primaryColor = settings.form_colors.primary_color || settings.default_settings.company_form_settings_primary_color;
      this.secondaryColor = settings.form_colors.secondary_color || settings.default_settings.company_form_settings_secondary_color;
      this.tertiaryColor = settings.form_colors.tertiary_color || settings.default_settings.company_form_settings_tertiary_color;
      this.welcomeMessage = settings.options.welcome_message || settings.default_settings.company_form_settings_welcome_message;
      this.pageTitle = settings.options.page.page_title;
      this.pageDesc = settings.options.page.page_description;
      this.successMessage = settings.success_message || settings.default_settings.company_form_settings_success_message;
      this.formBackgroundImage = settings.form_background_image || settings.default_settings.company_form_settings_form_background_image;
      this.pageBackgroundImage = settings.page_background_image || settings.default_settings.company_form_settings_page_background_image;

      // Yüklenme tamamlandı
      this.loading = false;
    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }
  },
  components: {
    DynamicForm,
  },
  methods: {
    startForm() {
      this.showWelcome = false;
    },
  }
};
</script>

<style>
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



.btn {
  display: inline-block;
  font-weight: 400;
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

.btn:hover {
  color: #fff !important;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}

</style>
