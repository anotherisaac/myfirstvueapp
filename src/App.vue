<template lang="pug">
  #app
    //pug format by html2jade.org NOTA: Considerar a futuro que NO es lo mismo el npm run dev al npm run build. Cuando se sube a Git, el proyecto requiere la carpeta dist porque en dev es virtual. (Llevar proyecto a produccion) HAY QUE QUITAR LA CARPETA DIST DEL GITIGNORE//
    //img(src='./assets/logo.png') DEVELOPMENT//
    img(src='https://anotherisaac.github.io/myfirstvueapp/dist/logo.png')
    h1 Top Artists by country
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
    spinner(v-show="isLoading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")

</template>

<script>
import Spinner from './components/Spinner.vue'
import Artist from './components/Artist.vue'

import getArtists from './api'

  export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Japan', value: 'japan'},
        {name: 'Thailand', value:'thailand' },
        {name: 'Argentina', value: 'argentina'}

      ],
      selectedCountry: 'argentina',
      isLoading: true
    }
  },
  components: {
    Artist:Artist,
    Spinner:Spinner
  },
  methods: {
    refreshArtists() {
      const self = this //IMPORTANTE
      this.isLoading = true; 
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists) {
        self.isLoading = false;
        self.artists = artists
      })
    }
  },

  mounted() {
    this.refreshArtists()
  } //Cuando ya se monto el componente en el DOM se ecutará este código. Profundizar ciclo de vida de los componentes
    
    
  ,
  watch: {
    selectedCountry() {
      this.refreshArtists()
    }  //Cuando haya un cambio en selectedCountry ejecuta esta funcion
      
    
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
li.artist 
  display inline-block
  margin 0 10px
a 
  color red !important
</style>
