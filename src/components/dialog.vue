<template>
  <div class="form">
    <h2>Диалог с {{wingman}} <button @click="backToList">назад к списку пользователей</button></h2>
    <div class="message-cont">
      <message :type="d.from !== this.login" :time="d.time" :text="d.text" v-for="d in this.data"></message>
      <hr>
      <footer>
        <input v-model="text" placeholder="Введите сообщение">
        <button @click="send">Отправить</button>
      </footer>
    </div>
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
    async send() {
      const date = new Date(), data = {
        from: this.login,
        to: this.wingman,
        text: this.text,
        time: String(date.getHours() + ':' + date.getMinutes())
      };
      this.text = "";
      this.data.push(data)
      await fetch('http://localhost:5000/api/dialogs', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      })
    },
    backToList() {
      this.$emit('backToList');
    },
    getKey(from, to) {
      return from > to? to + '$' + from : from + '$' + to;
    },
    async getMessages() {
      let json = await fetch(`http://localhost:5000/api/dialogs/${this.key}`);
      this.data = await json.json();
    }
  },
  props: {
    wingman: String,
    messages: Object
  },
  mounted() {
    this.login = localStorage['login'];
    this.key = this.getKey(this.wingman, this.login);
    this.getMessages()
    setTimeout(() => this.getMessages(), 5000);
  }
}
</script>

<style scoped>
h2 {
  color: white;
  display: flex; justify-content: center; align-items: center
}
hr {
  color: black;
  width: 100%;
}
.message-cont {
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  padding: 5px;
  width: 360px;
}
</style>