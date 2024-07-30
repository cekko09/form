<template>
  <div class="form-group">
    <label>{{ field.label }}</label>
    <br>
    <div v-for="(option, index) in field.form_field_options" :key="index" class="form-check">
      <input
        type="checkbox"
        :id="`${field.unique_id}_${index}`"
        :name="field.unique_id"
        :value="option.option_value"
        :checked="isChecked(option.option_value)"
         :required="field.is_required"
        @change="handleCheckboxChange(option.option_value, $event.target.checked)"
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
      const updatedValue = [...this.value];
      if (isChecked) {
        if (!updatedValue.includes(optionValue)) {
          updatedValue.push(optionValue);
        }
      } else {
        const index = updatedValue.indexOf(optionValue);
        if (index !== -1) {
          updatedValue.splice(index, 1);
        }
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
