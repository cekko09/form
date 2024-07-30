<template>
    <div class="rating">
        <label :for="field.form_field_type_id">{{ field.label }}</label>
      <span
        v-for="star in 5"
        :key="star"
        class="star"
        :class="{ filled: star <= hoverValue || star <= value }"
        @mouseover="setHoverValue(star)"
        @mouseleave="setHoverValue(0)"
        @click="updateValue(star)"
        
      >&#9733;</span>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      value: {
        type: Number,
        default: 0,
      },
      field: {
        type: Object,
        required: true,
      }
    },
    data() {
      return {
        hoverValue: 0,
      };
    },
    methods: {
      setHoverValue(value) {
        this.hoverValue = value;
      },
      updateValue(value) {
        this.$emit('input', value);
      }
    }
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
  }
  .star.filled {
    color: gold;
  }
  </style>
  