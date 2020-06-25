<template>
  <div>
    <label>{{label}}</label>
    <input
      :type="type ? type : 'text'"
      :value="value"
      @input="handleInput"
    >
  </div>
</template>

<script>
export default {
  props: {
    value: String,
    label: String,
    type: String
  },
  methods: {
    handleInput(event) {
      this.$emit("input", event.target.value);
    }
  }
};
</script>

<style scoped>
label {
  display: block;
}
</style>