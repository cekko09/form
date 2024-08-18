<template>
  <div class="form-group">
    <label :for="field.unique_id">{{ field.label }}</label>
    <div v-for="(option, index) in field.form_field_options" :key="index" class="form-check">
      <input
        type="radio"
        :id="`${field.unique_id}_${index}`"
        :name="field.unique_id"
        :value="option.option_value"
    @input="$emit('input', $event.target.value)"
        :required="field.is_required"
        class="form-check-input"
      />
      <label :for="`${field.unique_id}_${index}`" class="form-check-label">
        {{ option.option_label }}
      </label>
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
      type: [String, Number],
      default: '',
    },
  },
  data() {
      return {
        inputValue: this.value,
      };
    }
};
</script>

<style scoped>
.form-group {
  margin-bottom: 15px;
}

.form-check {
  margin-bottom: 10px;
}

.form-check-input {
  margin-right: 10px;
}

.form-check-label {
  margin-left: 5px;
}
</style>
