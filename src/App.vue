<template>
  <div>
    <ChatWindow v-if="open" />
    <div class="chatbot-widget" @click="open = !open">
      <ChatIcon width="40" height="40" v-if="!open" />
      <CloseIcon width="40" height="40" v-if="open" />
    </div>
  </div>
</template>

<script setup>
import ChatIcon from "@/components/icons/ChatIcon.vue";
import CloseIcon from "@/components/icons/CloseIcon.vue";
import ChatWindow from "@/components/chat/ChatWindow.vue";
import { ref } from "vue";

const open = ref(false);
</script>

