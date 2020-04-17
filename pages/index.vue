<template>
  <v-container fluid>
    <Search @channelChanged="channelId=$event" />
    <Player :vidId="stockId" />
    <List @idChanged="stockId=$event" @stockChanged="stockId=$event" :newId="channelId" />
  </v-container>
</template>

<script>
import Player from "../components/Player";
import List from "../components/List";
import Search from "../components/Search";

export default {
  data() {
    return {
      stockId: "",
      channelId: ""
    };
  },
  head() {
    return {
      title: "HomePage",
      meta: [
        {
          hid: "description",
          name: "desciption",
          content: "YouTube Videos Player"
        }
      ]
    };
  },

  components: {
    Search,
    Player,
    List
  }
};
</script>
