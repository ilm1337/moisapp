<script setup>
import { reactive } from "vue";
import { useUserStore } from "@/stores/user";
import { useRouter } from "vue-router";

const router = useRouter();
const state = reactive({ user: "" });
const { setUser } = useUserStore();

const handleSubmit = (e) => {
  e.preventDefault();
  setUser(state.user);
  router.push({ path: "/chat", replace: true });
};
</script>
<template>
  <form action="">
    <label for="name">Tu nombre:</label
    ><input type="text" name="user" v-model="state.user" />
    <button @click="handleSubmit">Entrar</button>
  </form>
</template>

<style></style>
