<template lang="pug">
  #app
    img(src="https://daguiheso.github.io/platzi-music/dist/logo.png")
    h1 Platzi Music

    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.name.toLowerCase()") {{ country.name }}

    spinner(v-show="loading")

    ul
      artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import getArtists from './api'
import Artist from './components/Artist'
import Spinner from './components/Spinner'
import countries from './api/countries'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries,
      selectedCountry: 'colombia',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists() {
      this.loading = true;
      this.artists = []
      const self = this
      getArtists(this.selectedCountry)
        .then(function(artists) {
          self.artists = artists
          self.loading = false;
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
