<template>
  <div :class="{'d-cont': isDark}" class="cont">
    <div class="flex">
      <smile @addSmile="addSmile" v-for="s in smilesList" :smile="s"></smile>
    </div>
  </div>
</template>

<script>
import Smile from "@/components/smile";
export default {
  name: "smilemenu",
  components: {Smile},
  props: {
    isDark: Boolean
  },
  data() {
    return {
      smiles: "😀 😃 😄 😁 😆 😅 😂 🤣  😊 😇 🙂 🙃 😉 😌 😍 🥰 😘 😗 😙 😚 😋 😛 😝 😜 🤪 🤨 🧐 🤓 😎 🤩 🥳 😏 😒 😞 😔 😟 😕 🙁 😣 😖 😫 😩 🥺 😢 😭 😤 😠 😡 🤬 🤯 😳 🥵 🥶 😱 😨 😰 😥 😓 🤗 🤔 🤭 🤫 🤥 😶 😐 😑 😬 🙄 😯 😦 😧 😮 😲 😴 🤤 😪 😵 🤐 🥴 🤢 🤮 🤧 😷 🤒 🤕 🤑 🤠",
      smilesList: []
    }
  },
  mounted() {
    this.smilesList = this.smiles.split(' ');
  },
  methods: {
    addSmile(code) {
      this.$emit('addSmile', code);
    }
  }
}
</script>

<style scoped>
.cont {
  padding: 6px;
  position: absolute;
  width: 300px;
  height: 200px;
  border-radius: 10px;
  z-index: 30;
  background-color: white;
  border: 1px solid gainsboro;
  right: 10px;
  bottom: 80px;
  overflow-y: scroll;
}
.d-cont {
  background-color: #1e1e1e;
  border: 1px solid black;
}

.close {
  width: 24px;
}

.close-cont {
  display: flex;
  justify-content: right;
  align-items: center;
}
.flex {
  flex-wrap: wrap;
  display: flex;
  align-items: center;
  justify-content: center;
}
.cont::-webkit-scrollbar {
  width: 0;
}
</style>