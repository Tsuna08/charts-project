<template>
  <v-expansion-panel>
    <v-expansion-panel-header>
      <slot name="panel-header" />
    </v-expansion-panel-header>
    <v-expansion-panel-content>
      <v-card-actions class="justify-space-between">
        <slot name="panel-content" />
      </v-card-actions>
    </v-expansion-panel-content>
  </v-expansion-panel>
</template>
