<template>
  <div class="chatbot-window">
    <div class="chatbot-window-header">
      <h3>Chatbot</h3>
      <button class="close" @click="$emit('closeWindow')">
        <CloseIcon />
      </button>
    </div>
    <div ref="parent" class="chatbot-window-body">
      <div class="panel" ref="child">
        <ul class="chat">
          <li
            v-for="(message, index) in data.messages"
            :class="message.direction"
            :key="index"
          >
            <ChatMessage :message="message.text" />
          </li>
        </ul>
        <ul class="chat" v-if="data.typing">
          <li>
            <img
              src="@/assets/loading.gif"
              alt="typing"
              width="40"
              class="typing"
            />
          </li>
        </ul>
        <SuggestButtons
          :buttons="data.buttons"
          @sendChoice="createUserMessage($event)"
        />
      </div>
      <chatForm
        @formSent="createUserMessage($event)"
        :settings="data.form"
        v-if="data.form !== null"
      />
    </div>
  </div>
</template>

<script setup>
import ChatMessage from "@/components/chat/ChatMessage.vue";
import CloseIcon from "@/components/icons/CloseIcon.vue";
import SuggestButtons from "@/components/chat/SuggestButtons.vue";
import { ref, onMounted } from "vue";
const parent = ref(null);
const child = ref(null);
const data = ref({
  messages: [],
  typing: false,
  buttons: [],
  form: null,
});
const errorMsg = {
  text: "Niečo sa pokazilo.:: Skús mi napísať neskôr.",
  direction: "bot",
};
const url = "https://robot.hurtis.sk";

function createUserMessage(userMessage) {
  data.value.messages.push({
    text: userMessage,
    direction: "user",
  });
  fetchData(userMessage);
}

function createBotMessage(botMessage) {
  data.value.messages.push({
    text: botMessage.text,
    direction: "bot",
  });
  data.value.buttons = botMessage.buttons;
  data.value.form = botMessage.form;
  data.value.typing = false;
  scrollToBottom();
}

function unifieString(myString) {
  let unifiedData = myString.toLowerCase().trim();
  unifiedData = unifiedData.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
  return unifiedData;
}

function fetchData(myData) {
  data.value.typing = true;
  const customerText = unifieString(myData);
  const question = {
    question: customerText,
    conversation: data.value.conversation,
  };
  fetch(url, {
    method: "POST",
    body: JSON.stringify(question),
    headers: {
      "Content-Type": "application/json",
    },
  })
    .then((response) => response.json())
    .then((data) => {
      createBotMessage(data);
    })
    .catch((error) => {
      console.error("Error:", error);
      createBotMessage(errorMsg);
    });
}
function scrollToBottom() {
  parent.value.scrollTop = child.value.scrollHeight;
}
onMounted(() => {
  fetchData("start");
});
</script>
