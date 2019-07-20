<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 Platzi Music

    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.name.toLowerCase()") {{ country.name }}

    ul
      artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import getArtists from './api'
import Artist from './components/Artist'
import countries from './api/countries'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries,
      selectedCountry: 'colombia'
    }
  },
  components: {
    Artist
  },
  methods: {
    refreshArtists() {
      const self = this
      getArtists(this.selectedCountry)
        .then(function(artists) {
          self.artists = artists
        })
    }
  },
  mounted() {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
      this.refreshArtists()
    }
  },
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
