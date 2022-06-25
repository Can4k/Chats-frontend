<template>
  <span @click="addSmile">
    {{smile}}
  </span>
</template>

<script>
export default {
  name: "smile",
  props: {
    smile: String
  },
  methods: {
    addSmile() {
      this.$emit('addSmile', {code: this.smile});
    }
  }
}
</script>

<style scoped>
span:hover {
  cursor: pointer;
  transform: scale(1.1);
}
span {
  user-select: none;
  padding: 1px;
}
</style>