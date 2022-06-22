<template>
  <div id="app">
    <span class="hello">
      <h1>{{isAuth? `Привет, ${login}!` : 'Вход'}}</h1>
      <button @click="exit" v-show="isAuth">Выйти</button>
    </span>
<!--    <user-card :key="i" v-for="i in body.length" :id="i" :login="body[i - 1].login"></user-card>-->
    <span class="enter" v-show="!isAuth">
      <input v-model="login" placeholder="Логин">
      <input v-model="password" placeholder="Пароль">
      <button @click="register">Зарегестрироваться</button>
      <button @click="enter">Войти</button>
    </span>
    <chat-dialog @backToList="isDialogOpen = false" v-if="isDialogOpen" :wingman="this.wingman"/>
    <user-list @openDialog="openDialog" v-if="isAuth && !isDialogOpen"/>
  </div>
</template>

<script>

import UserCard from "@/components/userCard";
import Message from "@/components/message";
import ChatDialog from "@/components/dialog"
import UserList from "@/components/userList";
export default {
  name: 'App',
  components: {
    UserList,
    Message,
    UserCard,
    ChatDialog
  },
  data() {
    return {
      body: {},
      login: "",
      password: "",
      isAuth: false,
      isDialogOpen: false,
      wingman: ""
    }
  },
  methods: {
    openDialog(data) {
      this.wingman = data.wingman;
      this.isDialogOpen = true;
    },
    check() {
      return this.login.length && this.password.length;
    },
    async enter() {
      if (!this.check()) {
        alert('Логин или пароль не могут быть пустыми');
        return;
      }
      const data = {
        login: this.login,
        password: this.password
      }
      let response = await fetch('http://localhost:5000/api/auth', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      });
      if (response.ok) {
        this.isAuth = true;
        localStorage.login = this.login;
        localStorage.password = this.password;
      } else {
        alert('Неверный логин или пароль');
      }
    },
    async register() {
      if (!this.check()) {
        alert('Логин или пароль не могут быть пустыми');
        return;
      }
      if (this.login.length > 20) {
        alert('Слишком длинный логин');
        return;
      }
      const data = {
        login: this.login,
        password: this.password
      }
      let response = await fetch('http://localhost:5000/api/register', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      });
      if (response.ok) {
        alert('Вы успешно зарегистрировались')
      } else {
        alert('Пользователь с таким логином уже существует');
      }
    },
    exit() {
      localStorage.clear();
      this.login = this.password = "";
      this.isAuth = false;
      this.isDialogOpen = false;
    },
  },
  mounted() {
    if (localStorage['login'] && localStorage['password']) {
      this.isAuth = true;
      this.login = localStorage.login;
      this.password = localStorage.password;
    }
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.hello {
  align-items: center;
}
body {
  background: #002137;
}
h1{
  color: white;
}
.enter {
  padding: 12px 16px;
  border-radius: 12px;
  background-color: white;
  color: black;
  box-shadow: 0 4px 12px 0 #0d234308;
  display: flex;
  flex-direction: column;
  align-items: center;
}
input {
  margin: 5px;
}
button {
  margin: 5px;
}
.hello {
  display: flex;
}
</style>
