<template>
  <span :class="{'d-info' : isDark}" class="info">
    <b>Здравствуйте, {{login}}</b>
    <span class="down">
      <button :class="{'d-btn': isDark}" @click="exit">Выйти</button>
    </span>
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
  props: {
    isDark: Boolean
  },
  mounted() {
    this.login = localStorage['login'];
  },
  methods: {
    exit() {
      this.$emit('exit');
    },
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
  border-radius: 7px;
  background-color: white;
  font-family: Calibri, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  box-shadow: 0 4px 12px 0 #0d234308;
}
.d-info {
  color: #949494;
  background-color: #2c2c2c;
}
.d-btn {
  background-color: #949494;
}
.d-btn:hover {
  background-color: #777777;
}
button {
  padding: 3px 6px;
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
.down {
  margin-top: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}
img {
  width: 24px;
  margin-left: 10px;
  padding: 2px;
  border-radius: 20px;
}
</style>