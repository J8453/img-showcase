<template>
  <div id="app">
    <div id="body">
      <router-view/>  
    </div>
    <div id="footer">
      © 2020
    </div>
  </div>
</template>

<style>
* {
  /*border: 1px solid #00000020;*/
}

* {
  box-sizing: border-box;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #fff;
}

button:hover {
  cursor: pointer;
}

#app {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
  background: linear-gradient(to bottom, #004585, #E5E5BE);
  height: 100vh;
  overflow: scroll;
}

#app>* {
  width: 100%;
  display: flex;
  justify-content: center;
}

#footer {
  font-size: 0.9em;
  align-self: flex-end;
  padding: 15px;
}
</style>
