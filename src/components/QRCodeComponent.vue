<template>
  <div class="container">
    <h1 class="title">{{ msg }}</h1>

    <div class="input-section">
      <label for="linkOption">Provide Link</label>
      <div class="input-container" v-if="selectedOption === 'link'">
        <input type="text" v-model="link" placeholder="Enter URL">
      </div>
    </div>

    <button class="generate-button" @click="generateQRCode">Generate QR</button>

    <button v-if="qrCode" class="download-button" @click="downloadQRCode">Download QR Code</button>

    <div v-if="qrCode" class="qr-code-container">
      <img :src="qrCode" alt="QR Code">
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import QRCode from 'qrcode'

defineProps({
  msg: String,
})

const selectedOption = ref('link')
const link = ref('')
const qrCode = ref(null)

const generateQRCode = async () => {
  if (!link.value.trim()) {
    console.error('Please enter a URL.')
    return
  }

  try {
    const url = await QRCode.toDataURL(link.value)
    qrCode.value = url
  } catch (error) {
    console.error('Error generating QR code:', error)
  }
}

const downloadQRCode = () => {
  const link = document.createElement('a')
  link.href = qrCode.value
  link.download = 'qrcode.png'
  link.click()
}
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: 0 auto;
  text-align: center;
  padding: 20px;
  background-color: #f0f0f0;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.title {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333;
}

.input-section {
  margin-bottom: 20px;
}

.input-container {
  display: inline-block;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 5px;
}

input[type="text"] {
  width: 200px;
  padding: 8px;
  border: none;
  border-radius: 5px;
}

.generate-button {
  background-color: #4caf50;
  color: #fff;
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.generate-button:hover {
  background-color: #45a049;
}

.download-button {
  background-color: #008CBA;
  color: #fff;
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.download-button:hover {
  background-color: #0073A6;
}

.qr-code-container {
  margin-top: 20px;
}

img {
  max-width: 100%;
  height: auto;
}
</style>
