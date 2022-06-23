<template>
  <span class="info">
    <b>Здравствуйте, {{login}}</b>
    <button @click="exit">Выйти</button>
  </span>
</template>

<script>
export default {
  name: "userInformation",
  data() {
    return {
      login: ""
    }
  },
  mounted() {
    this.login = localStorage['login'];
  },
  methods: {
    exit() {
      this.$emit('exit');
    }
  }
}
</script>

<style scoped>
.info {
  position: absolute;
  right: 0;
  top: 0;
  margin: 8px;
  padding: 8px;
  border-radius: 10px;
  background-color: white;
  font-family: Calibri, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  box-shadow: 0 4px 12px 0 #0d234308;
}
button {
  padding: 3px 6px;
  margin-top: 8px;
  font-size: 15px;
  border: none;
  background-color: #ffdada;
  border-radius: 5px;
  transition-duration: .1s;
}
button:hover {
  background-color: #ffaeae;
}
b {
  font-weight: 600;
}
</style>