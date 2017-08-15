<template lang="pug">
  #app
    img(src='https://adrianortiga.github.io/TopArtistas/dist/logo.png')
    h1 {{ msg }}
    h2 {{ msg2 }}
    h3 {{ msg3 }}
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul 
      artist(v-for="artist in artists" v-bind:artist="artist" :key="artist.mbid")
</template>

<script>
import artist from './components/artist.vue'
import spinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      msg: '¡Bienvenido!',
      msg2: 'Aquí están los Top artistas de diferentes paises',
      msg3: 'Esta hecha en Vue.js',
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'España', value: 'spain'},
        {name: 'Brasil', value: 'brazil'},
        {name: 'China', value: 'china'},
        {name: 'Francia', value: 'france'},
        {name: 'Alemania', value: 'germany'},
        {name: 'Italia', value: 'italy'},
        {name: 'Portugal', value: 'portugal'}, 
      ],
      selectedCountry: 'spain',
      loading: true
    }    
  },
  components: {
    artist,
    spinner
  },
  methods: {
    refreshArtists() {
      this.artists = []
      const self = this
      this.loading = true
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.loading = false
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
  }
}
</script>

<style lang="stylus">
#app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color #2c3e50
    margin-top 60px

h1, h2, h3
    font-weight normal

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px

a
    color #42b983
</style>
