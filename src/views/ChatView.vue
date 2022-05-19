<script setup>
import { reactive, onMounted } from "vue";
import { useUserStore } from "@/stores/user";
import { ref, set, onValue } from "firebase/database";
import { db } from "../firebase/config";
import ChatMessage from "@/components/ChatMessage.vue";

const state = reactive({
  newMsg: "",
  messages: "",
});

const { user } = useUserStore();

const readMessages = () => {
  const messagesRef = ref(db, "messages");
  onValue(messagesRef, (snapshot) => {
    const data = snapshot.val();
    console.log(data);
    state.messages = data;
    return data;
  });
};

const handleSubmit = (e) => {
  e.preventDefault();
  console.log(state.newMsg);
  set(ref(db, "messages/" + Date.now()), {
    user,
    msg: state.newMsg,
    createdAt: Date.now(),
  });
  state.newMsg = "";
};
onMounted(() => {
  readMessages();
});
</script>
<template>
  <div class="container">
    <h1 class="titulo">Chat 💬</h1>
    <h3>Hola {{ user }}</h3>
    <div class="chat">
      <ChatMessage
        v-for="message in state.messages"
        :key="Date(message.createdAt)"
        :user="message.user"
        :msg="message.msg"
        :createdAt="message.createdAt"
      />
    </div>
    <form @submit="handleSubmit" class="botoninput">
      <input type="text" v-model="state.newMsg" />
      <button>Enviar</button>
    </form>
  </div>
</template>

<style>
body {
  background: #25d366;
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.chat {
  display: flex;
  justify-content: flex-start;
  align-items: flex-end;
  flex-direction: column;
  background: white;
}
.titulo {
  text-align: center;
}

.chat {
  overflow: scroll;
  width: 100vw;
  height: 800px;
  border: 1px solid black;
}
input {
  width: 100%;
}
.botoninput {
  display: flex;
  width: 100vw;
}
</style>
