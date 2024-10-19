<template>
  <button type="button" :class="buttonClass" @click="handleClick">
    {{ label }}
  </button>
</template>

<script>
export default {
  name: "MyButton",
  props: {
    label: {
      type: [Number, String],
      required: true,
    },
    color: {
      type: String,
      default: "btn-light", // Default class if no color is provided
    },
  },
  methods: {
    handleClick() {
      this.$emit("click");
    },
  },
  computed: {
    buttonClass() {
      // Dynamically set the button class using the color prop
      return `btn ${this.color}`;
    },
  },
};
</script>

<style scoped></style>
