<template>
  <div id="app">
    <QRCodeComponent msg="QR Generator" />
  </div>
</template>

<script>
import QRCodeComponent from './components/QRCodeComponent.vue'
import QRCode from 'qrcode/lib/browser.js'


export default {
  components: {
    QRCodeComponent
  }
}
</script>

<style>
/* Add your global styles here */
</style>
