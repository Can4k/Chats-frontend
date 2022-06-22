<template>
  <div class="message" :class="[type? 'message-from' : 'message-to']">
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
    time: String
  }
}
</script>

<style scoped>
p {
  margin: 5px 0 0 0;
  font-weight: 1000;
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
  background-color: #496481;
  color: azure;
  align-self: flex-start;
}
.message-to {
  font-family: Arial, Helvetica, sans-serif;
  margin: 5px;
  padding: 10px 10px 5px 7px;
  border-radius: 10px 10px 0 10px;
  background-color: #2490ff;
  color: azure;
  align-self: flex-end;
}
</style>