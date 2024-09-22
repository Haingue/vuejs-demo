<script setup>
import { ref } from 'vue'
import HelloWorld from './components/HelloWorld.vue'

import { QrcodeStream, QrcodeDropZone, QrcodeCapture } from 'vue3-qrcode-reader'

let QrCodeObject = ref('QrcodeStream')
let barcode = ref('')

const onDecode = (decodePromise) => {
  decodePromise.then((decodeString) => {
    console.log('QR code decoded: ', decodeString)
    barcode.value = decodeString.content
  })
}
const resetBarcode = () => {
  barcode.value = undefined
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <div>
      <h1>QR Code reader</h1>
      <div>
        <select v-model="QrCodeObject">
          <option value="QrcodeStream">QrcodeStream</option>
          <option value="QrcodeDropZone">QrcodeDropZone</option>
          <option value="QrcodeCapture">QrcodeCapture</option>
        </select>
      </div>
      <div>
        <QrcodeStream v-if="QrCodeObject === 'QrcodeStream'" @detect="onDecode"></QrcodeStream>
        <QrcodeDropZone
          v-if="QrCodeObject === 'QrcodeDropZone'"
          @detect="onDecode"
        ></QrcodeDropZone>
        <QrcodeCapture v-if="QrCodeObject === 'QrcodeCapture'" @detect="onDecode"></QrcodeCapture>
      </div>
      <div>
        <center>
          <h2>
            <u>{{ barcode }}</u>
          </h2>
        </center>
      </div>
      <div>
        <button @click="resetBarcode">Reset</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
