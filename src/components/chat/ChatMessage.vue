<template>
  <div :class="'message-wrap'">
    <div class="avatar"></div>
    <div class="msg-content">
      <div
        class="msg"
        v-for="(text, index) in message.split('::')"
        :key="index"
      >
        {{ text }}
      </div>
    </div>
  </div>
</template>

<script setup>
const { message } = defineProps(["message"]);
</script>
