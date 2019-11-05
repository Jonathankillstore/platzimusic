<template lang="pug">
#app
  img(src='./assets/logo.png')  
  h2 PlastiziMusic
  ul
  h5 seleccione pais
  ul
   select(v-model="selectedCountry")
    option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
  spinner(v-show="loading")
  ul
    artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
  
     
</template>
<script>
  import Spinner from './components/Spinner.vue'
  import Artist from './components/Artist.vue'
  import getArtist from './api'
  
  export default {
    name: 'app',
    data () {
      return {         
        artists: [],
        countries: [
          {name: 'mexico', value: 'mexico'},
          {name: 'Chile', value: 'chile'},
          {name: 'Colombia', value: 'Colombia'},
        ],
        selectedCountry:'mexico',
        loading: true
      }
    },
    components: {
      Artist,
      Spinner
    },   
   
    // mounted: function () {
    //   const self = this
    //   getArtist()
    //    .then(function (artists) {
    //      self.artists = artists
    //    })
    // },
    methods:{
      refreshArtist(){
       
       const self = this
       this.loading = true
       this.artists = []
       getArtist(this.selectedCountry)
        .then(function (artists) {
          self.loading = false
          self.artists = artists
        })
      }
    },
     mounted: function() {
      this.refreshArtist()
    },
    watch: {
      selectedCountry(){
        this.refreshArtist()
        
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
