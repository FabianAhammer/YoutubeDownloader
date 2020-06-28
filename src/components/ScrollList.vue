<template>
  <div>
    <v-col cols="12">
      <v-text-field
        v-model="query"
        label="Search"
        single-line
        outlined
        :rules="[rules.required]"
        append-icon="mdi-send"
        @click:append="toggleSearch"
      ></v-text-field>
    </v-col>

    <div v-if="searchResults.length > 0">
      <v-col cols="12" v-for="(result, id) in searchResults" v-bind:key="id">
        <item :result="result"></item>
      </v-col>
    </div>
    <div v-if="searchResults.length == 0">
      <div v-if="startedQuery">
        <v-progress-circular :size="70" :width="7" color="orange" indeterminate></v-progress-circular>
      </div>
      <div v-if="!startedQuery">Search for a Song!</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import item from "./item.vue";

export default {
  name: "List",
  props: {},
  components: {
    item
  },
  data() {
    return {
      query: "",
      searchResults: [],
      startedQuery: false,
      startedDownload: false,
      rules: {
        required: value => !!value || "Required."
      }
    };
  },
  methods: {
    toggleSearch() {
      if (this.query.length > 0) {
        this.startedQuery = true;
        axios
          .get(
            "http://192.168.178.34:3000/searchSong?searchString=" + this.query,
            {
              headers: {
                "Access-Control-Allow-Origin": "*"
              },
              crossdomain: true
            }
          )
          .then(res => {
            this.searchResults = res.data;
          });
      }
    }
  },
  mounted() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.result {
  display: flex;
  margin: 10px;
  background-color: rgba(61, 61, 61, 0.705);
  height: 100px;
  border-radius: 10px;
}
.img {
  margin-top: 10px;
  margin-left: 10px;
  width: 80px;
  height: 80px;
  border-radius: 15px;
}
.text {
  margin-left: 10px;
  margin-top: 10px;
  color: white;
}
.title {
  margin-top: 10px;
}
.artist {
  margin-top: 10px;
}
.buttons {
  margin-left: auto;
  margin-right: 20px;
  margin-top: 30px;
}
</style>
