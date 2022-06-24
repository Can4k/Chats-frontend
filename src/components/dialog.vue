<template>
  <div :class="{'d-form': isDark}" class="form">
    <div :class="{'d-bnt': isDark}" @click="backToList" class="back">
      <img src="@/assets/arrow-left.svg" alt="назад к списку пользователей">
      Назад
    </div>
    <strong :class="{'d-str': isDark}">{{wingman}}{{mes}}</strong>
    <div :class="{'d-form': isDark}" class="over" ref="txt">
      <div :class="{'d-form': isDark}" class="message-cont">
        <message :is-dark="isDark" :type="d.from !== this.login" :time="d.time" :text="d.text" v-for="d in this.data"></message>
      </div>
    </div>
    <footer>
      <span class="foo">
        <textarea :class="{'d-txt' : isDark}" @keydown.ctrl.enter="send" v-model="text" placeholder="Введите сообщение, отправка на Ctrl+Enter."/>
        <img @click="send" class="send" alt="отправить" src="@/assets/send.svg">
      </span>
    </footer>
  </div>
</template>

<script>
import message from "@/components/message";
export default {
  name: "chat-dialog",
  components: {
    message
  },
  data() {
    return {
      key: "",
      data: {},
      login: "",
      text: "",
      mes: ""
    }
  },
  methods: {
    getTime() {
      let date = new Date();
      let str1 = date.getHours(), str2 = date.getMinutes();
      if (str1 < 10) str1 = '0' + str1;
      if (str2 < 10) str2 = '0' + str2;
      return str1 + ':' + str2;
    },
    async send() {
      if (this.text === "") {
        return;
      }
      const data = {
        from: this.login,
        to: this.wingman,
        text: this.text,
        time: this.getTime(),
      };
      this.text = "";
      this.data.push(data);
      this.toDown();
      await fetch('https://murmuring-beyond-69315.herokuapp.com/api/dialogs', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      });
    },
    backToList() {
      this.$emit('backToList');
    },
    getKey(from, to) {
      return from > to? to + '$' + from : from + '$' + to;
    },
    toDown() {
      setTimeout(() => this.$refs.txt.scrollTo(0, 1e16, 1e-10));
    },
    async getMessages(flag = false) {
      let json = await fetch(`https://murmuring-beyond-69315.herokuapp.com/api/dialogs/${this.key}`);
      let tmp = await json.json();
      if (tmp.length < this.data.length) {
        return;
      }
      if (!flag && tmp.length !== this.data.length) {
        this.mes = " [новое сообщение]"
      }
      this.data = tmp;
      if (flag && this.data) {
        this.toDown();
      }
    }
  },
  mounted() {
    this.login = localStorage['login'];
    this.key = this.getKey(this.wingman, this.login);
    this.getMessages(true);
    setInterval(() => this.getMessages(), 5000);
  },
  props: {
    wingman: String,
    messages: Object,
    isDark: Boolean
  },
}
</script>

<style scoped>
.back:hover {
  cursor: pointer;
}
.back {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: left;
  background-color: #f1f1f1;
  top: 0; left: 0;
  margin: 7px;
  border-radius: 8px;
  padding: 3px 7px 3px 2px;
}
h2 {
  color: black;
  display: flex; justify-content: center; align-items: center
}
hr {
  color: black;
  width: 100%;
  margin: 0;
}
.over {
  height: 100%;
  overflow-y: scroll;
}
.form {
  margin-top: 80px;
  position: absolute;
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  width: 90%;
  height: 85%;
  left: 50%;
  transform: translate(-50%);
  box-shadow: 0 4px 12px 0 #0d234308;
}
.message-cont {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  padding: 5px;
  background-color: white;
  margin-top: 0;
  margin-bottom: 10px;
}
.over::-webkit-scrollbar {
  width: 0;
}
img {
  width: 20px;
  margin-right: 3px;
  cursor: pointer;
}
strong {
  font-size: 25px;
  padding-top: 5px;
  padding-bottom: 5px;
  border-radius: 10px 10px 0 0;
  font-weight: 700;
  color: #424242;
}
.foo {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-bottom: 10px;
  padding-top: 10px;
  border-radius: 0 0 10px 10px;
}
textarea {
  padding: 10px;
  resize: none;
  width: 85%;
  height: 50px;
  border: none;
  border-radius: 10px;
  background-color: #f1f1f1;
}
textarea:focus {
  outline: none;
}
.send {
  width: 24px;
  border-radius: 16px;
  padding: 4px;
  background-color: #f1f1f1;
  border: 1px solid #c0c0c0;
  margin-left: 5px;
}
button {
  padding: 0;
}
.d-form {
  background-color: #1e1e1e;
}
.d-str {
  color: #949494;
}
.d-txt {
  background-color: #444444;
  color: #c0c0c0;
}
.d-bnt {
  background-color: #949494;
}
@media screen and (min-width: 1000px){
  strong {
    font-size: 35px;
  }
  .back {
    font-size: 20px;
    margin-top: 10px;
  }
  .form {
    width: 60%;
  }
}
</style>