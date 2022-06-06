<template>
  <div id="app">
    <main>
      <div class="background" v-bind:style="{'background-image': 'url(' + img_link + ')'}" >
        <div class='ripple-background'>
        <div class='circle xxlarge shade1'></div>
        <div class='circle xlarge shade2'></div>
        <div class='circle large shade3'></div>
        <div class='circle mediun shade4'></div>
        <div class='circle small shade5'></div>
        </div>

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
        <p class="under-text stroke" v-if="typeof weather.main != 'undefined'"> {{Math.round(weather.main.temp) - 273}}°C</p>
        <p class="stroke under-text" v-if="typeof weather.main != 'undefined'"> {{weather.weather[0].main}}</p>
        <p class="stroke small-text" v-if="typeof weather.main != 'undefined'"> Humidity: {{weather.main.humidity}}%</p>
        <p class="stroke small-text" v-if="typeof weather.main != 'undefined'"> Pressure: {{weather.main.pressure}}hPa</p>
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
      weather: {},
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
      }).catch(err => console.log('error', err))
      },
    fetchDatasAndProcess(){
      clearTimeout(this.timer)
      this.timer = setTimeout(() => {
        if (this.weather.cod != "400" || this.weather.cod!= "404"){
          this.fetchRandomPhotoByQuery()
          this.fetchCityByName()
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
body{
  background: #3399ff;
}


.circle{
  position: absolute;
  border-radius: 50%;
  background: white;
  animation: ripple 15s infinite;
  box-shadow: 0px 0px 1px 0px #508fb9;
}

.small{
  width: 200px;
  height: 200px;
  left: -100px;
  bottom: -100px;
}

.medium{
  width: 400px;
  height: 400px;
  left: -200px;
  bottom: -200px;
}

.large{
  width: 600px;
  height: 600px;
  left: -300px;
  bottom: -300px;
}

.xlarge{
  width: 800px;
  height: 800px;
  left: -400px;
  bottom: -400px;
}

.xxlarge{
  width: 1000px;
  height: 1000px;
  left: -500px;
  bottom: -500px;
}

.shade1{
  opacity: 0.2;
}
.shade2{
  opacity: 0.5;
}

.shade3{
  opacity: 0.7;
}

.shade4{
  opacity: 0.8;
}

.shade5{
  opacity: 0.9;
}

@keyframes ripple{
  0%{
    transform: scale(0.8);
  }
  
  50%{
    transform: scale(1.2);
  }
  
  100%{
    transform: scale(0.8);
  }
}

</style>