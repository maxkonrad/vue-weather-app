<template>
  <div id="app">
    <main>
      <div class="background">

      <div class="card">
        <div class="search-box">
          <input 
            type="text"
            class="search-bar" 
            placeholder="Search..."
            v-model="query"
            @keyup="fetchDatasAndProcess"
            />
        </div>
        <p class="country-name stroke">{{query.toUpperCase()}}</p>
        <p class="under-text stroke">{{Math.round(weather.main.temp) - 273}}</p>
        <p class="stroke under-text">{{weather.weather[0].main}}</p>
        <p class="stroke small-text">Humidity: {{weather.main.humidity}}%</p>
        <p class="stroke small-text">Pressure: {{weather.main.pressure}}hPa</p>
      </div>
      
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
      url: 'https://api.openweathermap.org/data/2.5/weather', //https://api.openweathermap.org/data/2.5/weather?q={city name}&appid={API key}
      api_key: "6dffb22c8ce89a4da256a6de232b13e0",
      query: 'london',
      img_link: '',
      weather: '',
      timer: null,
      transition: false,
    }
  },
  methods: {
    fetchRandomPhotoByQuery(){
      fetch(this.USPLS_URL + `?query=${this.query}` + `&client_id=${this.USPLS_ACCESS_KEY}`)
      .then(Response => Response.json())
      .then(json => {this.img_link = json.results[Math.floor(Math.random() * 9)].urls.regular
      
      })
      .catch(err => console.log("error", err))
    },
    fetchCityByName(){
      fetch(this.url + `?q=${this.query}` + `&appid=${this.api_key}`).then(Response => Response.json()).then(json=> {
        this.weather = json
      })
      },
    fetchDatasAndProcess(){
      clearTimeout(this.timer)
      this.timer = setTimeout(() => {
          this.fetchCityByName()
          if (this.weather.cod != "400" || this.weather.cod!= "404"){
          this.fetchRandomPhotoByQuery()
          }
          else{
            return
          }
      }, 1500)
    }
  
  },
  created() {
    this.fetchDatasAndProcess()
  },
}
</script>
<style >
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@600;700&display=swap');
.country-name{
  font-family: 'Oswald', sans-serif;
  margin: 25px 5px;
  font-size: 2.5rem;
}
body{
  background-color: blueviolet;
}
.background{
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
  height: 100vh;
}
.card{
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.search-bar{
  padding: 15px;
  border-radius: 15px;
  border: none;
  background-color: rgba(255, 17, 0, 0.575);
  text-align: center;
  color: white;
  text-transform: uppercase;
  margin-top: 17px;
}
.stroke{
  text-shadow: 2px 0 0 #fff, -2px 0 0 #fff, 0 2px 0 #fff, 0 -2px 0 #fff, 1px 1px #fff, -1px -1px 0 #fff, 1px -1px 0 #fff, -1px 1px 0 #fff;
}
.under-text{
  text-align: center;
  font-size: 3rem;
  font-family: 'Oswald', sans-serif;
}
p{
  margin: 10px 0px !important;
}
.small-text{
  text-align: center;
  font-size: 1.5rem;
  font-family: 'Oswald', sans-serif;
}
</style>