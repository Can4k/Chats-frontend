<template>
  <div id="app">
    <span class="enter" v-show="!isAuth">
       <span class="hello">
         <h1>{{isAuth? `Привет, ${login}!` : 'Вход'}}</h1>
       </span>
      <span class="inputs">
        <input v-model="login" placeholder="Логин">
        <input type="password" v-model="password" placeholder="Пароль">
      </span>
      <span class="buttons-container">
        <button id="enter" @click="enter">Войти</button>
        <button id="register" @click="register">Зарегестрироваться</button>
      </span>
    </span>
    <user-information @exit="this.exit" v-if="isAuth"/>
    <chat-dialog @backToList="isDialogOpen = false" v-if="isDialogOpen" :wingman="this.wingman"/>
    <user-list @openDialog="openDialog" v-if="isAuth && !isDialogOpen"/>
  </div>
</template>

<script>

import UserCard from "@/components/userCard";
import Message from "@/components/message";
import ChatDialog from "@/components/dialog"
import UserList from "@/components/userList";
import UserInformation from "@/components/userInformation";
export default {
  name: 'App',
  components: {
    UserInformation,
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
      let response = await fetch('https://serene-spire-46051.herokuapp.com/api/auth', {
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
      let response = await fetch('https://serene-spire-46051.herokuapp.com/api/register', {
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
html {
  background-color: #f3f3f2;
}
* {
  box-sizing: border-box;
  font-family: Avenir, Helvetica, Arial, sans-serif
}
</style>

<style scoped>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
}
.hello {
  align-items: center;
}
h1 {
  color: black;
  margin-bottom: 7px;
  margin-top: 0;
  font-size: 25px;
}
.enter {
  width: 300px;
  padding: 12px 16px;
  border-radius: 12px;
  background-color: white;
  color: black;
  box-shadow: 0 4px 12px 0 #0d234308;
  flex-direction: column;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  left: 50%;
  top: 40%;
  transform: translate(-50%, -50%);
}
input {
  margin: 5px;
  padding: 7px 10px;
  border-radius: 5px;
  border: none;
  background-color: #f6f6f6;
  transition-duration: .3s;
  width: 270px;
  font-size: 12px;
}
input:focus{
  outline: none;
  border: none;
  transform: translate(3px);
}
button {
  padding: 5px 10px;
  margin: 5px;
  border: none;
  background-color: #ececec;
  border-radius: 5px;
}
.hello {
  display: flex;
}
.buttons-container {
  display: flex;
  flex-direction: revert;
  align-items: center;
  justify-content: center;
}

#register {
  background-color: #b9e1ff;
  transition-duration: .1s;
}
#enter {
  background-color: #d9f3e7;
  transition-duration: .1s;
}
@media screen and (min-width: 700px){
  .enter {
    width: 480px;
    padding: 10px;
  }
  #enter:hover {
    background-color: #aee7c7;
  }
  #register:hover {
    background-color: #95c8ff;
  }
  input {
    font-size: 16px;
    width: 450px;
    border-radius: 10px;
    padding: 10px;
    padding-left: 13px;
  }
  button {
    font-size: 16px;
    border-radius: 8px;
    padding: 10px 12px;
  }
  h1 {
    font-size: 35px;
    margin: 0;
  }
  .inputs {
    margin: 10px 0 10px 0;
  }
}
</style>
