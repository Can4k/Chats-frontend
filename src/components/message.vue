<template>
  <div class="message" :class="[type? 'message-from' : 'message-to', isDark? 'dark' : 'light']">
    {{text}}
    <p>{{time}}</p>
  </div>
</template>

<script>
export default {
  name: "message",
  props: {
    type: Boolean,
    text: String,
    time: String,
    isDark: Boolean
  }
}
</script>

<style scoped>
p {
  margin: 5px 0 0 1px;
  font-size: 10px;
}
.message {
  word-wrap: anywhere;
  max-width: 50%;
  text-align: left;
}

.message-from {
  font-family: Arial, Helvetica, sans-serif;
  margin: 5px;
  padding: 10px 10px 7px 5px;
  border-radius: 10px 10px 10px 0;
  background-color: rgba(82, 107, 187, 0.20);
  color: #2c2c2c;
  align-self: flex-start;
}
.message-to {
  font-family: Arial, Helvetica, sans-serif;
  margin: 5px;
  padding: 10px 10px 5px 7px;
  border-radius: 10px 10px 0 10px;
  background-color: rgba(36, 144, 255, 0.20);
  color: #2c2c2c;
  align-self: flex-end;
}
.dark {
  color: #d5d5d5;
}
</style>