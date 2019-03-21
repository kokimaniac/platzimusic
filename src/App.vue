<template lang="pug">
#app
  img(src='dist/music_logo.png')
  h1 Platzimusic
  select(v-model="selectedCountry")
    option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
  spinner(v-show="loading")
  ul
    artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")

</template>

<script>
import Artist from './components/Artist'
import Spinner from './components/Spinner'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name: 'Argentina', value: 'argentina'},
        { name: 'Bolivia', value: 'bolivia'},
        { name: 'Chile', value: 'chile'},
        { name: 'EEUU', value: 'united states'},
        { name: 'England', value: 'united kingdom'}
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist, Spinner
  },
  methods: {
    refreshArtists (){
      const self = this
      this.loading = true
      getArtists(this.selectedCountry)
        .then(function(artists){
          self.loading=false
          self.artists = artists
        })
    }
  },
  mounted (){
    this.refreshArtists()
  },
  watch: {
    selectedCountry(){
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
  color gray
  margin-top 60px

h1, h2
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
