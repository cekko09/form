<template>
  <div class="form-group">
    <label>{{ field.label }}</label>
    <br>
    <div v-for="(option, index) in field.form_field_options" :key="index" class="form-check">
      <input
        type="checkbox"
        :id="`${field.unique_id}_${index}`"
        :name="field.unique_id"
        :value="option.option_label"
        :checked="isChecked(option.option_label)"
       
        @change="handleCheckboxChange(option.option_label, $event.target.checked)"
        class="form-check-input"
      />
      <label :for="`${field.unique_id}_${index}`" class="form-check-label">{{ option.option_label }}</label>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    field: {
      type: Object,
      required: true,
    },
    value: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    isChecked(optionValue) {
      return this.value.includes(optionValue);
    },
    handleCheckboxChange(optionValue, isChecked) {
      let updatedValue = [...this.value];
      if (isChecked) {
        if (!updatedValue.includes(optionValue)) {
          updatedValue.push(optionValue);
        }
      } else {
        updatedValue = updatedValue.filter(val => val !== optionValue);
      }
      this.$emit('input', updatedValue);
    },
  },
};
</script>

<style scoped>
.form-group {
  margin-bottom: 15px;
}
.form-check {
  display: inline-block;
  margin-right: 10px;
}
.form-check-input {
  margin-right: 5px;
}
</style>
