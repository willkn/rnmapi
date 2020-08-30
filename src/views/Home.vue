<template>
  <div>
    <h1>RNM API</h1>
    <input id="search" placeholder="name" v-model="urlElements.name" />
    <br />
    <input id="search" placeholder="location" v-model="urlElements.location" />
    <br />
    <input id="search" placeholder="gender" v-model="urlElements.gender" />
    <br />
    <button @click.prevent="urlBuilder">Search</button>
    <div class="container">
      <div class="row">
        <div v-for="object in characterInfo" :key="object.id">
          <div class="col-sm-4">
            <Cell class="cell" :object="object"></Cell>
          </div>
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
        name: "",
        id: "",
      },
    };
  },
  methods: {
    apiCall(searchTerm) {
      this.$store.commit("startLoading");
      if (searchTerm != null) {
        // this.characterInfo = [];
        // axios(searchTerm).then((response) =>
        //   response.forEach((element) => {
        //     this.characterInfo.push({
        //       id: element.id,
        //       name: element.name,
        //       gender: element.gender,
        //       location: element.location,
        //       img: element.image,
        //     });
        //   })
        // );
      } else {
        axios(
          "https://rickandmortyapi.com/api/character/" + this.numList
        ).then((response) => this.populateCell(response.data));
      }
      this.$store.commit("stopLoading");
    },
    urlBuilder() {
      var baseurl = "https://rickandmortyapi.com/api/character/?";
      var paramString = new URLSearchParams({
        name: this.urlElements.name,
        location: this.urlElements.location,
        gender: this.urlElements.gender,
      });
      var constructedURL = baseurl + paramString;
      // this.apiCall(constructedURL);
      // Useless but demonstrates that the URL construction works.
      console.log(constructedURL);
    },
    populateCell(apiResult) {
      apiResult.forEach((element) => {
        this.characterInfo.push({
          id: element.id,
          name: element.name,
          gender: element.gender,
          location: element.location,
          img: element.image,
        });
      });
    },
  },
  created() {
    for (var i = 0; i < 6; i++) {
      this.numList.push(Math.floor(Math.random() * 670 + 1));
    }
    this.apiCall();
    this.$store.commit("stopLoading");
  },
};
</script>
