<template lang="pug">
  #app
    img(src='https://luisreyes98.github.io/vuejs-basic-course/dist/logo.png')
    h1 Platzi Music
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
      //- es necesario por vue que las estructuras repetitivas de lista tengan un key en caso de que algun valor cambie


</template>

<script>
import getArtists from './api';
import Artist from './components/Artists.vue';
import Spinner from './components/Spinner.vue';

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name: 'Argentina', value: 'argentina'},
        { name: 'Colombia', value: 'colombia'},
        { name: 'España', value: 'spain'},
        { name: 'Venezuela', value: 'venezuela'},
      ],
      selectedCountry: 'venezuela',
      loading: true,
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
    loadArtists(){
      const self = this;
      this.loading = true;
      this.artists = [];
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.loading = false;
          self.artists = artists;
        })

    }
  },
  mounted() {
    this.loadArtists();
  },
  watch: {
    selectedCountry(){
      this.loadArtists();

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
