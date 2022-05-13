<template>
  <div>
    <label>{{ label }}</label>
    <v-radio-group :value="value" @change="updateValue($event)" row>
      <v-radio v-for="radio in items" :key="radio" :label="radio" :value="radio"></v-radio>
    </v-radio-group>
  </div>
</template>
<script>
export default {
  props: ['items', 'value', 'label'],
  methods: {
    updateValue(value) {
      this.$emit('input', value)
    }
  }
}
</script>
