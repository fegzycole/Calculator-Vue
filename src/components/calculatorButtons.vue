<template>
  <div>
    <button
      v-for="(value, index) in buttons"
      :key="index"
      @click="handleClick(value)"
    >
      {{ value }}
    </button>
  </div>
</template>

<script>
export default {
  props: ['buttons'],
  methods: {
    handleClick(value) {
      this.$emit("perform-action", value);
    }
  }
};
</script>
