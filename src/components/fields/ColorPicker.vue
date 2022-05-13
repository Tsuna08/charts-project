<template>
  <div>
    <label>{{ label }}</label>
    <v-color-picker
      :value="value"
      class="colorPickerInput"
      dot-size="25"
      hide-canvas
      hide-inputs
      hide-sliders
      mode="hsla"
      show-swatches
      swatches-max-height="200"
      @input="updateValue($event)"
    ></v-color-picker>
  </div>
</template>
<script>
export default {
  props: ['value', 'label'],
  methods: {
    updateValue(value) {
      this.$emit('input', value)
    }
  }
}
</script>
<style lang="sass" scoped>
.colorPickerInput
  height: 4.5em
  margin-left: -12px
</style>
