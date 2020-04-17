<template>
  <v-row class="justify-center">
    <v-col cols="12">
      <v-card-actions style="padding:0" class="justify-center">
        <iframe
          align-self="center"
          id="player"
          type="text/html"
          width="100%"
          height="500"
          :src="`https://www.youtube.com/embed/${id}?enablejsapi=1&origin=http://example.com`"
          frameborder="0"
        ></iframe>
      </v-card-actions>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: ["vidId"],
  data() {
    return {
      id: this.vidId
    };
  },
  watch: {
    vidId: function() {
      this.id = this.vidId;
    }
  }
};
</script>