<template>
  <div id="app">
    <qrreader msg="QR Code generator"/>
  </div>
</template>

<script>
import qrreader from './components/qr_reader.vue'

export default {
  name: 'App',
  components: {
    qrreader
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
