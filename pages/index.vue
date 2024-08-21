<template>
  <div>
    <div class="logo_container text-center">
        
        <img :src="logo" class="img-fluid" height="300" width="300" alt="">
      </div>
    <div class="page_details">
      <h1 :style="{ color: tertiaryColor }"> {{ pageTitle }}</h1>
      <p :style="{ color: tertiaryColor }"> {{ pageDesc }}</p>
    </div>
    <DynamicForm />
  </div>
</template>

<script>
import DynamicForm from '~/components/DynamicForm.vue';

export default {
  data() {
    return {
      pageTitle: '',
      pageDesc: '',
      logo: null,
    };
  },

  async created() {
    try {
      const settingsResponse = await fetch('/form_settings.json');
      if (!settingsResponse.ok) {
        throw new Error('Settings data network response was not ok');
      }
      const settings = await settingsResponse.json();

      this.logo = settings.logo || settings.default_settings.company_form_settings_logo;
      this.primaryColor = settings.form_colors.primary_color || settings.default_settings.company_form_settings_primary_color;
      this.secondaryColor = settings.form_colors.secondary_color || settings.default_settings.company_form_settings_secondary_color;
      this.tertiaryColor = settings.form_colors.tertiary_color || settings.default_settings.company_form_settings_tertiary_color;
      this.pageTitle = settings.options.page.page_title;
      this.pageDesc = settings.options.page.page_description;
  

    } catch (error) {
      console.error('There was a problem with the fetch operation:', error);
    }
  },
  components: {
    DynamicForm,
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

h1, p {
  text-align: center;
  margin-top: 20px;
}
</style>
