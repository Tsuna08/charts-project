<template>
  <v-card>
    <v-card-actions :class="className">
      <slot name="card-actions" />
    </v-card-actions>
    <slot name="card-content" />
  </v-card>
</template>
<script>
export default {
  props: ['className']
}
</script>
