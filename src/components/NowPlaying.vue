<template>
  <div id="app">
    <div class="background-container" :style="backgroundStyle"></div>
    <div
      v-if="player.playing"
      class="now-playing"
      :class="getNowPlayingClass()"
    >
      <div class="now-playing__cover">
        <img
          :src="player.trackAlbum.image"
          :alt="player.trackTitle"
          class="now-playing__image"
        />
      </div>
      <div class="now-playing__details">
        <h1 class="now-playing__track" v-text="player.trackTitle"></h1>
        <h2 class="now-playing__artists" v-text="getTrackArtists"></h2>
      </div>
    </div>
    <div v-else class="now-playing" :class="getNowPlayingClass()">
      <h1 class="now-playing__idle-heading">No music is playing 😔</h1>
    </div>
  </div>
</template>

<script>
import * as Vibrant from 'node-vibrant'

import props from '@/utils/props.js'

export default {
  name: 'NowPlaying',

  props: {
    auth: props.auth,
    endpoints: props.endpoints,
    player: props.player
  },

  data() {
    return {
      pollPlaying: '',
      playerResponse: {},
      gifUrl: 'https://' // Default GIF URL
    };
  },

  computed: {
    backgroundStyle() {
      return {
        backgroundImage: `url(${this.gifUrl})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        position: 'absolute',
        top: '0',
        left: '0',
        width: '100%',
        height: '100%',
        zIndex: '-1',
      };
    }
  },

  watch: {
    'player.trackAlbum.image': function (newImage) {
      // Example: Update GIF based on album art or track
      this.gifUrl = `https://example.com/gifs/${this.player.trackTitle}.gif`;
    }
  },

  methods: {
    getNowPlayingClass() {
      // Existing logic for class names
      return 'now-playing-active';
    },
    getTrackArtists() {
      // Example helper method to format artist names
      return this.player.trackArtists.join(', ');
    }
  }
};
</script>

<style>
.background-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  overflow: hidden;
}

.now-playing {
  position: relative;
  z-index: 1;
  text-align: center;
  color: #fff;
}

.now-playing__cover {
  margin: auto;
}

.now-playing__details {
  margin-top: 20px;
}
</style>
