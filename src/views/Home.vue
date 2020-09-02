<template>
  <div>
    <h1>RNM API</h1>
    <form>
      <input id="search" placeholder="name" v-model="urlElements.name" />
      <br />
      <input id="search" placeholder="location" v-model="urlElements.location" />
      <br />
      <input id="search" placeholder="gender" v-model="urlElements.gender" />
      <br />
      <button @click.prevent="urlBuilder" @keydown.enter="urlBuilder">Search</button>
    </form>
    <div class="container">
      <div class="row">
        <div v-for="object in characterInfo" :key="object.id">
          <div class="col-sm-4">
            <Cell class="cell" :object="object"></Cell>
          </div>
        </div>
        <div id="error">
          <h2 v-if="this.error != ''">{{ error }}, please try again</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Cell from "@/components/Cell";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Cell,
  },
  data() {
    return {
      numList: [],
      characterInfo: [],
      urlElements: {
        gender: "",
        location: "",
        id: "",
      },
      url: "https://rickandmortyapi.com/api/character/",
      error: "",
    };
  },
  methods: {
    apiCall(searchTerm) {
      this.$store.commit("startLoading");
      if (searchTerm != null) {
        this.characterInfo = [];
        axios(searchTerm)
          .then((response) => {
            this.populateCell(response.data.results);
            this.error = "";
          })
          .catch((error) => {
            this.error = error;
          });
      } else {
        axios(this.url + this.numList).then((response) =>
          this.populateCell(response.data)
        );
      }
      this.$store.commit("stopLoading");
    },
    urlBuilder() {
      var baseurl = this.url + "?";
      var paramString = new URLSearchParams({
        name: this.urlElements.name,
        location: this.urlElements.location,
        gender: this.urlElements.gender,
      });
      var constructedURL = baseurl + paramString;
      this.apiCall(constructedURL);
    },
    populateCell(apiResult) {
      for (const result of apiResult) {
        this.characterInfo.push({
          id: result.id,
          name: result.name,
          gender: result.gender,
          location: result.location,
          img: result.image,
        });
      }
    },
  },
  created() {
    for (var i = 0; i < 6; i++) {
      this.numList.push(Math.floor(Math.random() * 670 + 1));
    }
    this.apiCall();
  },
};
</script>
