<template>
  <div class="friends">
    <h3>Список пользователей</h3>
    <b @click="openDialog(i.login)" v-show="i.login !== login" v-for="i in userList">{{i.login}}</b>
  </div>
</template>

<script>
export default {
  name: "userList",
  data() {
    return {
      userList: {},
      login: "",
    }
  },
  methods: {
    async getUserList() {
      const res = await fetch('https://serene-spire-46051.herokuapp.com/api/users');
      if (res.ok) {
        this.userList = await res.json();
      } else {
        alert('Не удалость получить список пользователей');
      }
    },
    openDialog(wing) {
      this.$emit('openDialog', {wingman: wing});
    },
    exit() {
      this.$emit('exit');
    }
  },
  async mounted() {
    this.login = localStorage['login'];
    setTimeout(() => this.getUserList(), 5000);
  }
}
</script>

<style scoped>
h3 {
  color: black;
  margin: 2px 0 10px 0;
  font-family: Calibri, sans-serif;
  font-size: 23px;
}
b {
  margin: 5px;
  padding: 10px;
  background-color: #f1f1f1;
  border-radius: 10px;
  color: black;
  cursor: pointer;
  transition-duration: .3s;
  font-weight: 400;
  font-family: Calibri, sans-serif;
}
.friends{
  margin-top: 90px;
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  padding: 10px;
  color: black;
  width: 300px;
  position: absolute;
  left: 50%;
  transform: translate(-50%);
  box-shadow: 0 4px 12px 0 #0d234308;
}
b:hover {
  transform: translate(5px);
}
@media screen and (min-width: 700px){
  .friends {
    width: 600px;
  }
  b {
    font-size: 20px;
  }
  h3 {
    font-size: 30px;
  }
}
</style>