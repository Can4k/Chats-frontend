<template>
  <div class="form">
    <div @click="backToList" class="back">
      <img src="@/assets/arrow-left.svg" alt="назад к списку пользователей">
      Назад
    </div>
    <strong>{{wingman}}</strong>
    <div class="message-cont" ref="txt">
      <message :type="d.from !== this.login" :time="d.time" :text="d.text" v-for="d in this.data"></message>
    </div>
    <footer>
      <span class="foo">
        <textarea v-model="text" placeholder="Введите сообщение"/>
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
      text: ""
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
      await fetch('https://serene-spire-46051.herokuapp.com/api/dialogs', {
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
      setTimeout(() => this.$refs.txt.scrollTo(0, 1e16), .3);
    },
    async getMessages(flag = false) {
      let json = await fetch(`https://serene-spire-46051.herokuapp.com/api/dialogs/${this.key}`);
      this.data = await json.json();
      if (this.data && flag) {
        setTimeout(() => this.toDown());
      }
    }
  },
  props: {
    wingman: String,
    messages: Object
  },
  mounted() {
    this.login = localStorage['login'];
    this.key = this.getKey(this.wingman, this.login);
    this.getMessages(true)
    setTimeout(() => this.getMessages(), 5000);
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
.form {
  margin-top: 80px;
  position: absolute;
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  width: 360px;
  left: 50%;
  transform: translate(-50%);
  box-shadow: 0 4px 12px 0 #0d234308;
}
.message-cont {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  overflow-y: scroll;
  max-height: 480px;
  padding: 5px;
  background-color: white;
  margin-top: 0;
  margin-bottom: 10px;
}
.message-cont::-webkit-scrollbar {
  width: 0;
}
img {
  width: 20px;
  margin-right: 3px;
  cursor: pointer;
}
strong {
  margin-top: 6px;
  font-size: 25px;
  margin-bottom: 5px;
  border-bottom: 1px solid gainsboro;
  padding-bottom: 5px;
}
.foo {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-bottom: 10px;
}
textarea {
  padding: 7px;
  resize: none;
  width: 270px;
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
@media screen and (min-width: 700px){
  .form {
    width: 600px;
  }
  textarea {
    font-size: 15px;
    height: 80px;
    width: 530px;
  }
}
</style>