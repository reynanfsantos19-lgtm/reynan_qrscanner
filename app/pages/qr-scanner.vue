<template>
  <div>
    <h1>QR Scanner Page</h1>

    <v-card class="mx-auto" max-width="600">
      <v-card-text class="d-flex flex-column align-center ga-4">
        <video ref="videoRef" class="qr-video" autoplay playsinline muted></video>

        <p class="text-subtitle-1 mb-0">Result: {{ result || 'No result yet' }}</p>

        <div class="d-flex ga-3">
          <v-btn color="green" @click="startScanner" :disabled="isScanning">
            Start Scanner
          </v-btn>
          <v-btn color="red" @click="stopScanner" :disabled="!isScanning">
            Stop Scanner
          </v-btn>
        </div>
      </v-card-text>
    </v-card>
  </div>
</template>

<script lang="ts" setup>
definePageMeta({
  layout: 'default',
  middleware: 'auth',
})

import QrScanner from 'qr-scanner'

const result = ref('')
const videoRef = ref<HTMLVideoElement | null>(null)
const isScanning = ref(false)

let scanner: QrScanner | null = null

const startScanner = async () => {
  if (!videoRef.value) return

  try {
    if (scanner) {
      await scanner.start()
      isScanning.value = true
      return
    }

    const hasCamera = await QrScanner.hasCamera()
    if (!hasCamera) {
      result.value = 'Unable to start camera. Check your camera permission and try again.'
      isScanning.value = false
      return
    }

    scanner = new QrScanner(
      videoRef.value,
      (scanResult) => {
        result.value = scanResult.data
        console.log('Scanned result:', scanResult.data)
      },
      {
        preferredCamera: 'environment',
        highlightScanRegion: true,
        highlightCodeOutline: true,
      },
    )

    await scanner.start()
    isScanning.value = true
  } catch (error) {
    console.error('Camera start failed:', error)
    result.value = 'Unable to start camera. Check your camera permission and try again.'
    isScanning.value = false

    if (scanner) {
      scanner.destroy()
      scanner = null
    }
  }
}

const stopScanner = () => {
  if (!scanner) return

  scanner.stop()
  scanner.destroy()
  scanner = null
  isScanning.value = false
}

onBeforeUnmount(() => {
  stopScanner()
})
</script>

<style scoped>
.qr-video {
  width: 100%;
  max-width: 400px;
  min-height: 280px;
  border-radius: 12px;
  background: #000;
  object-fit: cover;
}
</style>