<template>
  <div class="main">
    <h1>Willkommen zu Tesseract.Js!</h1>
    <p>
      Es ist Basiscode vorhanden, welcher nicht bearbeitet werden sollte. Stellen, an welchen du im Code Hand anlegen darfst, sind mit einem TODO-Kommentar markiert.
    </p>
    <input type="file" @change="loadFile">
    <p id="result" v-if="result">{{result}}</p>

    <!-- AUFGABE 3
    <button @click="createCameraElement">Kamera öffnen</button>
    <button @click="takePhoto">Foto machen</button>
    <video  v-show="!isPhotoTaken" ref="camera" :width="450" :height="337.5" autoplay></video>
    <canvas v-show="isPhotoTaken" id="photoTaken" ref="canvas" :width="450" :height="337.5"></canvas>
    -->
  </div>
</template>

<script>
import Tesseract from 'tesseract.js';

export default {
  name: 'MainView',
  data() {
    return {
      result: null,
      isLoading: null,
      isPhotoTaken: false
    }
  },
  methods: {
    recognize(base64){
      //Diese Funktion wird automatisch aufgerufen, sobald eine Datei hochgeladen wurde
      this.progress = 0;
      this.result = null;

      let imageBuffer = Buffer.from(base64, "base64")
      //TODO: Aufgabe 1 und 2
    },
    takePhoto() {
      if(!this.isPhotoTaken) {
        this.isShotPhoto = true;

        const FLASH_TIMEOUT = 50;

        setTimeout(() => {
          this.isShotPhoto = false;
        }, FLASH_TIMEOUT);
      }

      this.isPhotoTaken = !this.isPhotoTaken;

      const context = this.$refs.canvas.getContext('2d');
      console.log(this.$refs.canvas.toDataURL())
      context.drawImage(this.$refs.camera, 0, 0, 450, 337.5);

      //TODO: AUFGABE 3
      //let imageToRecognize = this.$refs.canvas.toDataURL().substring(22);
    },
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
    createCameraElement() {
      this.isLoading = true;

      const constraints = (window.constraints = {
        audio: false,
        video: true
      });


      navigator.mediaDevices
          .getUserMedia(constraints)
          .then(stream => {
            this.isLoading = false;
            this.$refs.camera.srcObject = stream;
          })
          .catch(error => {
            this.isLoading = false;
            alert("Der Browser unterstützt keine Webcam", error);
          });
    }
  }
}
</script>

<style scoped>
.main {
  position: absolute;
  background: rgb(255,255,255);
  padding: 10px;
  text-align: center;
  width: 50vw;
  height: 50vh;
  top: 50%;
  left: 50%;
  margin-top: -25vh;
  margin-left: -25vw;
}

h1, p {
  font-family: monospace;
}

p {
  text-align: left;
}

#result {
  color: red;
}
</style>
