<script setup>
import { ref, onMounted } from 'vue'

const transcript = ref('')
const isRecording = ref(false)

const Recognition = window.speechRecognition || window.webkitSpeechRecognition
const vr = new Recognition()

onMounted(() => {
  vr.continuous = true
  vr.interimResults = true

  vr.onstart = () => {
    isRecording.value = true
      console.log(isRecording.value)
  }

  vr.onend = () => {
    isRecording.value = false
  }

  vr.onresult = (event) => {
    const trans = Array.from(event.results)
                    .map(result => result[0])
                    .map(result => result.transcript)
                    .join('')

    transcript.value = trans
  }
  
})

const ToggleMic = () => {
  if(isRecording.value) {
    vr.stop()
  } else {
    vr.start()
  }
}

</script>

<template>
  <div class="app">
    <button :class="`mic`" @click="ToggleMic">Record</button>
    <div class="transcript" v-text="transcript"></div>
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body {
  background: #654485;
  color: #fff;
}
</style>
