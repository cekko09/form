<template>
  <div class="rating">
    <label :for="field.form_field_type_id">{{ field.label }}</label>
    <span
      v-for="star in stars"
      :key="star.sort_order"
      class="star"
      :class="{ filled: star.sort_order <= hoverValue || star.sort_order <= currentValue }"
      @mouseover="setHoverValue(star.sort_order)"
      @mouseleave="resetHoverValue"
      @click="updateValue(star.option_value, star.sort_order)"
    >
      &#9733;
    </span>
  </div>
</template>

<script>
export default {
  props: {
    field: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      hoverValue: 0,
      currentValue: this.field.default_value || 0,
      stars: Array.from({ length: 5 }, (_, index) => ({
        sort_order: index + 1,
        option_value: index + 1,
      })),
    };
  },
  methods: {
    setHoverValue(value) {
      this.hoverValue = value;
    },
    resetHoverValue() {
      this.hoverValue = 0;
    },
    updateValue(value, sort_order) {
      this.currentValue = sort_order;
      this.$emit('input', value);
    },
  },
};
</script>

<style scoped>
.rating {
  display: flex;
}
.star {
  font-size: 2rem;
  color: #ccc;
  cursor: pointer;
  transition: color 0.2s;
}
.star.filled {
  color: gold;
}
</style>
