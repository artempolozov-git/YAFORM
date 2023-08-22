<template>
  <div id="app">
    <main-form></main-form>
  </div>
</template>

<script>
import MainForm from "./components/MainForm";
export default {
  name: 'App',
  components: {
      MainForm
  }
}
</script>

<style>
  html, body {
  height: 100%;
}
body {
  font-family: Montserrat;
  font-size: 16px;
  font-weight: normal;
  margin: 0;
}
#app {
  display: flex;
  justify-content: center;
  padding: 60px 30px;
  box-sizing: border-box;
  min-height: 100%;
  max-width: 1920px;
  background-color: #ffffff;
}
@font-face {
  font-family: Montserrat;
  src: url('./assets/Montserrat-SemiBold.ttf');
  font-weight: bold;
}
@font-face {
  font-family: Montserrat;
  src: url('./assets/Montserrat-Medium.ttf');
  font-weight: normal;
}
@font-face {
  font-family: Montserrat;
  src: url('./assets/Montserrat-Regular.ttf');
  font-weight: 500;
}
</style>
