<template>
  <div class="main">
    <h1>Willkommen zu Tesseract.Js!</h1>
    <p>
      Hier ist Platz für alle Arbeitsaufträge, du kannst alles am Systen anpassen.
    </p>
    <input type="file" @change="loadFile">
    <progress :value="progress" max="1"></progress>
    <p id="result" v-if="result">{{result}}</p>
  </div>
</template>

<script>
import Tesseract from 'tesseract.js';

export default {
  name: 'MainView',
  data() {
    return {
      progress: 0,
      result: null
    }
  },
  methods: {
    loadFile(input){
      if(input.target.files.length > 0){
        const file = input.target.files[0];
        const reader = new FileReader();

        reader.onload = (fileLoadedEvent) => {
          //Entferne data:image/png;base64 Präfix
          this.recognize(fileLoadedEvent.target.result.substring(22))
        }
        reader.readAsDataURL(file)
      }
    },
    recognize(base64){
      let imageBuffer = Buffer.from(base64, "base64")
      this.progress = 0;
      this.result = null;

      Tesseract.recognize(
          imageBuffer,
          'eng',
          { logger: m => this.progress = m.progress }
      ).then(({ data: { text } }) => {
        console.log(text);
        this.result = text
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main {
  background: rgb(200,200,200);
  padding: 10px;
  text-align: center;
  width: 50vw;
  height: 50vh;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -25vh;
  margin-left: -25vw;
}

h1, p {
  font-family: sans-serif;
}

#result {
  color: red;
}
</style>
