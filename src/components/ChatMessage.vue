<script setup>
import { reactive } from "vue";
const props = defineProps({
  msg: String,
  user: String,
  createdAt: Number,
});
const state = reactive({ date: new Date(props.createdAt).toLocaleString() });
</script>
<template>
  <div class="msg">
    {{ state.date }} - <span v-if="props.user">{{ props.user }}</span>
    <span v-else>Anónimo</span> : {{ props.msg }}
  </div>
</template>
<style>
.msg {
  background: lightblue;
  display: inline-block;
  padding: 1em;
  border-radius: 20px;
  margin: 1em;
}
</style>
