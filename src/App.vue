<template lang="pug">
#app
  img(src='https://ecaminux.github.com/vuemusic/dist/logo.png')
  h1 Vue music
  h2 Un projecto realizado en el 
    a(href="https://goo.gl/zPaIiU" target="_blank") curso de Platzi
  select(v-model="selectedCountry") 
    option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
  spinner(v-show="loading")
  ul
    //li(v-for="artist in artists") {{artist.name}}
    artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
  

</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Argentina', value: 'argentina'},
        {name: 'Colombia', value: 'colombia'},
        {name: 'Ecuador', value: 'ecuador'},
        {name: 'España', value: 'spain'},
        {name: 'Peru', value: 'peru'},
      ],
      selectedCountry: 'argentina',
      loading: true,
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    refreshArtists(){
      const self = this
      self.loading = true
      getArtists(this.selectedCountry)
      .then( function (artists){
         self.loading = false
         self.artists = artists
      })
    }
  },
  mounted: function(){
    this.refreshArtists()
  },
  watch: {
    selectedCountry: function(){
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
    color #666
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
