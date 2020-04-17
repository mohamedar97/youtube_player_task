<template>
  <v-row justify="space-between" class="list-container">
    <v-col cols="2" v-for="video in videos" :key="video.id">
      <v-card>
        <div class="vid">
          <v-icon large class="play-icon">mdi-play-circle-outline</v-icon>

          <div @click="play" :data-id="video.id" class="overlay"></div>
          <nuxt-link to="/">
            <img style="width:175px" :src="video.thumbnail" />
          </nuxt-link>
        </div>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import axios from "axios";
export default {
  props: { newId: { type: String, default: "" } },
  data() {
    return {
      videos: [],
      spares: [],
      channelId: this.newId
    };
  },
  watch: {
    newId: function() {
      this.channelId = this.newId;
      this.fetchVids(this.channelId);
    }
  },
  methods: {
    play: function(event) {
      let targetid = event.target.dataset.id;
      function getCurrentElement(x) {
        return x.id == targetid;
      }
      let currentIndex = this.videos.findIndex(getCurrentElement);
      let randIndex = Math.floor(Math.random() * this.spares.length);
      let newVid = this.spares[randIndex];
      let currentVid = this.videos[currentIndex];

      this.videos.splice(currentIndex, 1, newVid);
      this.spares.splice(randIndex, 1, currentVid);

      this.$emit("idChanged", targetid);
    },
    fetchVids: async function(id = "UC-4KnPMmZzwAzW7SbVATUZQ") {
      this.videos = [];
      this.spares = [];
      let thumbnailLink = `https://www.googleapis.com/youtube/v3/activities?part=snippet&channelId=${id}&maxResults=10&key=AIzaSyDbOu60-rEewnvYo6A4W6E5QTs555lzY_U`;
      let idLink = `https://www.googleapis.com/youtube/v3/activities?part=contentDetails&channelId=${id}&maxResults=10&key=AIzaSyDbOu60-rEewnvYo6A4W6E5QTs555lzY_U`;

      try {
        const res1 = await axios.get(thumbnailLink);
        const res2 = await axios.get(idLink);
        let thumbs = res1.data.items;
        let ids = res2.data.items;
        for (let i = 0; i < 10; i++) {
          let temp = { thumbnail: "", id: "" };
          const thumb = thumbs[i];
          const id = ids[i];
          temp.thumbnail = thumb.snippet.thumbnails.default.url;
          temp.id = id.contentDetails.upload.videoId;
          if (i < 4) {
            this.videos.push(temp);
          } else {
            this.spares.push(temp);
          }
          this.$emit("stockChanged", this.videos[0].id);
        }
      } catch (err) {}
    }
  },
  created() {
    this.fetchVids();
  }
};
</script>
<style scoped>
.list-container .vid {
  position: relative;
}
.list-container .vid .overlay {
  position: absolute;
  width: 106%;
  height: 100%;
  cursor: pointer;
}
.list-container .vid .play-icon {
  position: absolute;
  top: 38%;
  right: 44%;
}
</style>