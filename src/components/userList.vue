<template>
  <div :style="style" :class="{'d-friend': isDark}" class="friends">
    <h3 :class="{'d-h3': isDark}">Список пользователей</h3>
    <div class="search">
      <input :class="{'d-inp': isDark}" v-model="filterInput" @input="filter" placeholder="Поиск">
    </div>
    <transition-group name="list">
      <b :key="i._login" :class="{'d-b': isDark}" @click="openDialog(i.login)" v-show="i.login !== login"
         v-for="i in userList">{{ i.login }}</b>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "userList",
  data() {
    return {
      _userList: {},
      userList: {},
      login: "",
      style: {
        opacity: 0
      },
      filterInput: ""
    }
  },
  props: {
    isDark: Boolean
  },
  methods: {
    filter() {
      this.userList = this._userList.filter((word) => {
        return (this.filterInput.toLowerCase() === word._login.substring(0, this.filterInput.length));
      })
    },
    async getUserList() {
      const res = await fetch('https://murmuring-beyond-69315.herokuapp.com/api/users');
      if (res.ok) {
        this._userList = await res.json();
        this.userList = [].concat(this._userList);
      } else {
        alert('Не удалость получить список пользователей');
      }
    },
    openDialog(wing) {
      this.$emit('openDialog', {wingman: wing});
      localStorage['lastDialog'] = wing;
    },
    exit() {
      this.$emit('exit');
    }
  },
  async mounted() {
    this.login = localStorage['login'];
    setTimeout(() => this.getUserList());
    if (localStorage['lastDialog']) {
      this.openDialog(localStorage['lastDialog']);
    }
  },
  watch: {
    userList: function () {
      this.style.opacity = 1;
    }
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

.friends {
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
  transition-duration: .3s;
}

.d-friend {
  background-color: #2c2c2c;
}

.d-h3 {
  color: #949494;
}

.d-b {
  background-color: black;
  color: #949494;
}

b:hover {
  background-color: #dcdcdc;
}

.loader {
  width: 18px;
  margin-left: 10px;
  animation: bounce 0.5s;
}

.bounce-enter-active {
  animation: bounce-in 0.5s;
}

.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}

h3 {
  display: flex;
  align-items: center;
  justify-content: center;
}

.search {
  display: flex;
  justify-content: center;
  align-items: center;
}

input {
  padding: 8px;
  width: 50%;
  border: none;
  border-radius: 5px;
  background-color: #f6f6f6;
  margin-bottom: 10px;
  font-size: 15px;
}

input:focus {
  outline: none;
}

.list-move, /* apply transition to moving elements */
.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
}

.d-inp {
  background-color: black;
  color: grey;
}

@media screen and (min-width: 700px) {
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