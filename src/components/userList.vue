<template>
  <div class="friends">
    <h3>Выбери собеседника</h3>
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
      const res = await fetch('http://localhost:5000/api/users');
      if (res.ok) {
        this.userList = await res.json();
      } else {
        alert('Не удалость получить список пользователей');
      }
    },
    openDialog(wing) {
      this.$emit('openDialog', {wingman: wing});
    }
  },
  async created() {
    this.login = localStorage['login'];
    setTimeout(() => this.getUserList(), 5);
  }
}
</script>

<style scoped>
h3 {
  color: #2f4794;
  margin: 2px 0 10px 0;
}
b {
  font-family: Arial, Helvetica, sans-serif;
  margin: 5px;
  padding: 10px;
  background-color: #2f4794;
  border-radius: 10px;
  color: azure;
  cursor: pointer;
  transition-duration: .3s;
}
.friends{
  margin: 0 2px 0 2px;
  border-radius: 5px;
  display: flex;
  width: 360px;
  flex-direction: column;
  align-items: center;
  background-color: white;
  padding: 10px;
}
b:hover {
  transform: translate(5%);
}
</style>