<template>
  <div class="container-fluid">
    <h1>Hi There</h1>
    <SearchBar @termChange="onTermChange"></SearchBar>
    <div class="row">
      <VideoDetail v-bind:video="selectedVideo" class="col-md-8"></VideoDetail>
      <VideoList v-bind:videos="videos" @videoSelect="onVideoSelect" class="col-md-4"></VideoList>
    </div>
  </div>
</template>

<script>
import SearchBar from "./components/SearchBar";
import VideoList from "./components/VideoList";
import VideoDetail from "./components/VideoDetail";
import axios from "axios";

const API_KEY = "AIzaSyDyHLzjqfCEOcV1PGX7XXWDI0_C769bqlU";

export default {
  name: "App",
  components: {
    SearchBar,
    VideoList,
    VideoDetail
  },

  data() {
    return { videos: [], selectedVideo: null };
  },

  methods: {
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
    async onTermChange(searchTerm) {
      const response = await axios.get(
        "https://www.googleapis.com/youtube/v3/search",
        {
          params: {
            key: API_KEY,
            type: "video",
            part: "snippet",
            q: searchTerm
          }
        }
      );

      this.videos = response.data.items;
    }
  }
};
</script>
