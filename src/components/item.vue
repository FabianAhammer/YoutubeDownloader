<template>
  <div>
    <v-card dark>
      <v-img
        :src="result.thumbnail"
        class="white--text align-end"
        gradient="to bottom, rgba(0,0,0,.8), rgba(0,0,0,.5)"
        height="200px"
      >
        <v-card-title dark v-text="result.title"></v-card-title>
        <v-card-subtitle v-text="result.artist"></v-card-subtitle>
      </v-img>

      <v-card-actions>
        <v-spacer></v-spacer>
        <div v-if="downloadState == 0">
          <v-btn icon v-on:click="triggerDownload(result.link,result.title)">
            <v-icon>mdi-progress-download</v-icon>
          </v-btn>
        </div>

        <div v-if="downloadState != 0">
          <v-btn icon>
            <v-icon large style="color:green">mdi-check</v-icon>
          </v-btn>
        </div>
      </v-card-actions>
    </v-card>

    <v-overlay :value="overlay" :opacity="0.8">
      <v-card
        dark
        :src="result.thumbnail"
        style="width:80vw;height:40vh"
        gradient="to bottom, rgba(0,0,0,.8), rgba(0,0,0,.5)"
      >
        <v-card-title>MP3 {{result.title}} is beeing downloaded!</v-card-title>
        <v-card-text>
          <v-progress-circular style="color:red" indeterminate size="64"></v-progress-circular>
        </v-card-text>
      </v-card>
      <v-btn
        @click="overlay = false"
        style="display:flex; margin-top: 20px; justify-self:center; align-self:center"
        color="error"
      >
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-overlay>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Item",
  props: {
    result: Object
  },
  data() {
    return {
      downloadState: 0,
      overlay: false
    };
  },
  methods: {
    triggerDownload(link, title, event) {
      this.overlay = true;
      console.log(event);
      axios({
        method: "post",
        url: "http://192.168.178.34:3000/downloadSong",
        responseType: "arraybuffer",
        data: {
          link: link
        }
      })
        .then(response => {
          this.forceFileDownload(response, title);
        })
        .catch(() => console.log("error occured"));
    },
    forceFileDownload(response, title) {
      const url = window.URL.createObjectURL(new Blob([response.data]));
      const link = document.createElement("a");
      link.href = url;
      link.setAttribute("download", title + ".mp3"); //or any other extension
      document.body.appendChild(link);
      link.click();
      this.downloadState = 1;
      this.overlay = false;
    }
  }
};
</script>