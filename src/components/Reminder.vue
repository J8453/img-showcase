<template>
  <div class="reminder">
    <div class="reminder__msg">
      {{ msg }}  
    </div>
  </div>
</template>

<script>
export default {
  name: 'Reminder',
  props: {
    msg: String
  }
}
</script>

<style scoped>
.reminder {
  position: absolute;
  width: 100%;
  text-align: center;
  display: flex;
  justify-content: center;
}

.reminder__msg {
  flex: 0 1 700px;
  background-color: #ffffff99;
  border: 1px solid #00000020;
  border-radius: 4px;
  padding: 10px;
  color: rgb(189, 41, 30);
  font-size: 0.9em;
  opacity: 1;
  transition: opacity 2s ease-in;
}

.fade-leave-active {
  transition: opacity 4s;
}
.fade-leave-to {
  opacity: 0;
}
</style>
