<template>
  <div class="container">
    <SearchBar @termChange='onTermChange'></SearchBar>
    <div class="row">
      <div class="col-md-8">
        <VideoDetail :video='selectedVideo'></VideoDetail>
      </div>
      <div class="col-md-4">
        <VideoList @videoSelect='onVideoSelect' :videos='videos'></VideoList>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

const API_KEY = 'AIzaSyA2xjqK3p9-d7bvlZDwSaHurPDhKbPXlls';

export default {
  name: 'App',
  components: {
    'SearchBar': () => import('./components/SearchBar'),
    'VideoList': () => import('./components/VideoList'),
    'VideoDetail': () => import('./components/VideoDetail')
  },
  data() {
    return {
      videos: [],
      selectedVideo: null
    };
  },
  methods: {
    onTermChange(searchTerm) {
      axios.get('https://www.googleapis.com/youtube/v3/search', {
        params: {
          key: API_KEY,
          type: 'video',
          part: 'snippet',
          q: searchTerm
        }
      }).then(response => {
        this.videos = response.data.items;
      });
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
    }
  }
};
</script>
