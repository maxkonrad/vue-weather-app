<template>
  <div id="app">
    <main>
      <p class="country-name">{{query}}</p>
      <div class="search-box">
        <input 
          type="text"
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keydown.enter="fetchRandomPhotoByQuery"
          />
        


        <img :src="img_link" class="background-image">
      </div>
    </main>
  </div>
</template>
<script>
export default {
  name: 'app',
  data() {
    return {
      USPLS_ACCESS_KEY: 'XjEXS7Bgzz23z4VX3ldbgTjxLjTCrNLijrt1I8Pz5ho',
      USPLS_URL: 'https://api.unsplash.com/search/photos/',
      url: 'https://api.openweathermap.org/data/2.5/weather',
      query: '',
      img_link: ''
    }
  },
  methods: {
    fetchRandomPhotoByQuery(){
      fetch(this.USPLS_URL + `?query=${this.query}` + `&client_id=${this.USPLS_ACCESS_KEY}`)
      .then(Response => Response.json())
      .then(json => this.img_link = json.results[Math.floor(Math.random() * 9)].urls.small)
    },
    fetchCityByName(){
      
    }
  },
}
</script>
<style scoped >
  
</style>