<template>
  <form @submit.prevent="formSubmited()">
    <input
      class="textarea"
      v-model="myText"
      :type="settings.type"
      :placeholder="settings.placeholder"
    />
  </form>
</template>

<script setup>
import { ref } from "vue";
const { settings } = defineProps(["settings"]);
const emit = defineEmits(["formSent"]);
const myText = ref("");
function formSubmited() {
  if (myText.value != "") {
    emit("formSent", myText.value);
    myText.value = "";
  }
}
</script>