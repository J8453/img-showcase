<template>
  <span
    class="deleteBtn"
    v-on:click="handleDelete">
    X
  </span>
</template>

<script>
export default {
  name: 'DeleteBtn',
  methods: {
    handleDelete () {
      if (!confirm('確定要刪除嗎？')) return
      this.$emit('delete')
    }
  }
}
</script>

<style scoped>
.deleteBtn {
  position: absolute;
  top: 0px;
  right: 0px;
  margin-right: 5px;
  color: rgb(189, 41, 30);
  font-size: 2em;
  font-weight: bold;
}

.deleteBtn:hover {
  cursor: pointer;
}
</style>
