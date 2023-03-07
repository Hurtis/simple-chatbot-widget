<template>
  <div class="sugest-wrapper">
    <span
      class="btn"
      v-for="(btn, index) in buttons"
      :key="index"
      @click="$emit('sendChoice', btn)"
    >
      {{ btn }}
    </span>
  </div>
</template>

<script setup>
const { buttons } = defineProps(["buttons"]);
</script>