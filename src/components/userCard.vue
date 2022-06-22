<template>
  <span class="cont">
    {{login}}
  </span>
</template>

<script>
export default {
  name: "userCard",
  props: {
    login: String,
    id: Number
  }
}
</script>

<style scoped>
.cont {
  padding: 12px 16px;
  border-radius: 12px;
  background-color: white;
  color: black;
  box-shadow: 0 4px 12px 0 #0d234308;
  font-size: 18px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-weight: 700;
}
</style>