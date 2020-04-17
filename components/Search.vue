<template>
  <v-row align="center">
    <v-col class="d-flex" cols="12" sm="6">
      <v-select v-model="newId" :items="items" label="Choose Channel"></v-select>
    </v-col>
    <v-col class="text-center" cols="12" sm="4">
      <div class="my-2">
        <v-btn @click="changeChannel">View Videos</v-btn>
      </div>
    </v-col>
  </v-row>
</template>
<script>
export default {
  data() {
    return {
      items: [
        { text: "AJ+ كبريت", value: "UC-4KnPMmZzwAzW7SbVATUZQ" },
        { text: "3Blue1Brown", value: "UCYO_jab_esuFRV4b17AJtAw" },
        { text: "Tasty", value: "UCJFp8uSYCjXOMnkUyb3CQ3Q" },
        { text: "Fireship", value: "UCsBjURrPoezykLs9EqgamOA" },
        { text: "RealLifeLore", value: "UCP5tjEmvPItGyLhmjdwP7Ww" }
      ],
      newId: ""
    };
  },
  methods: {
    changeChannel: function(event) {
      this.$emit("channelChanged", this.newId);
    }
  }
};
</script>