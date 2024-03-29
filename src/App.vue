<template>
  <div id="app">
    <main>
      <div class="background">
        <div
          class="loading"
          v-if="videos.total_results == 0 || videos.status == '404'"
        ></div>
        <video
          v-else
          v-if="typeof weather.main != 'undefined'"
          autoplay
          muted
          loop
          id="cityVideo"
          :src="videoLink"
          type="video/mp4"
        ></video>

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
          <p class="country-name stroke">{{ query.toUpperCase() }}</p>
          <p
            class="under-text stroke"
            v-if="typeof weather.main != 'undefined'"
          >
            {{ Math.round(weather.main.temp) - 273 }}°C
          </p>
          <p
            class="stroke under-text"
            v-if="typeof weather.main != 'undefined'"
          >
            {{ weather.weather[0].main }}
          </p>
          <p
            class="stroke small-text"
            v-if="typeof weather.main != 'undefined'"
          >
            Humidity: {{ weather.main.humidity }}%
          </p>
          <p
            class="stroke small-text"
            v-if="typeof weather.main != 'undefined'"
          >
            Pressure: {{ weather.main.pressure }}hPa
          </p>
        </div>
      </div>
    </main>
  </div>
</template>
<script>
export default {
  name: "app",
  data() {
    return {
      PEXELS_KEY: "563492ad6f91700001000001889bf528fc3b452ab8fa0da328d1d9a3", //"https://api.pexels.com/videos/search?query=nature&per_page=1"
      PEXELS_URL: "https://api.pexels.com/videos/search",
      url: "https://api.openweathermap.org/data/2.5/weather", //https://api.openweathermap.org/data/2.5/weather?q={city name}&appid={API key}
      api_key: "6dffb22c8ce89a4da256a6de232b13e0",
      query: "london",
      weather: {},
      videos: {},
      timer: null,
      videoLink: '',
    };
  },
  methods: {
     fetchRandomVideoByQuery() {
      const headers = new Headers();
      headers.append("Authorization", this.PEXELS_KEY);
      if (this.isMobile()) {
        const request = new Request(
          this.PEXELS_URL +
            `?query=${this.query}` +
            "&per_page=1" +
            "&orientation=portrait" +
            "&size=medium",
          {
            method: "GET",
            headers,
            mode: "cors",
            cache: "default",
          }
        );
        if (this.query.trim().length > 0) {
          fetch(request)
            .then((res) => res.json())
            .then((json) => {
              this.videos = json;
              if (json.total_results == 0) {
                this.videoLink = "";
              } else {
                json.videos[0].video_files.forEach(
                  (videoFile) => {
                    if (videoFile.width == 1920 || videoFile.width == 1080) {
                      this.videoLink = videoFile.link;
                    }
                  }
                );
              }
            });
        }
      } else {
        const request = new Request(
          this.PEXELS_URL +
            `?query=${this.query}` +
            "&per_page=40" +
            "&orientation=landscape" +
            "&size=medium",
          {
            method: "GET",
            headers,
            mode: "cors",
            cache: "default",
          }
        );
        if (this.query.trim().length > 0) {
           fetch(request)
            .then((res) => res.json())
            .then((json) => {
              this.videos = json;
              if (json.total_results == 0) {
                this.videoLink = "";
              } else {
                json.videos[0].video_files.forEach(
                  (videoFile) => {
                    if (videoFile.width == 1920 || videoFile.width == 1080) {
                      this.videoLink = videoFile.link;
                    }
                  }
                );
              }
            });
        }
      }
    },
    fetchCityByName() {
      if (this.query.trim().length > 0) {
        fetch(this.url + `?q=${this.query}` + `&appid=${this.api_key}`)
          .then((Response) => Response.json())
          .then((json) => {
            this.weather = json;
          });
      }
    },
    fetchDatasAndProcess() {
      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.fetchRandomVideoByQuery();
        this.fetchCityByName();
      }, 1500);
    },
    isMobile() {
      if (
        /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
          navigator.userAgent
        )
      ) {
        return true;
      } else {
        return false;
      }
    },
  },
  created() {
    this.fetchDatasAndProcess();
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Oswald:wght@600;700&display=swap");
html,
body {
  margin: 0;
  height: 100%;
  overflow: hidden;
}
.country-name {
  font-family: "Oswald", sans-serif;
  margin: 25px 5px;
  font-size: 2.5rem;
}
body {
  background-color: blueviolet;
}
.background {
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center;
  height: 100vh;
}
.card {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
.search-bar {
  padding: 15px;
  border-radius: 15px;
  border: none;
  background-color: rgba(255, 17, 0, 0.575);
  text-align: center;
  color: white;
  text-transform: uppercase;
  margin-top: 17px;
}
.stroke {
  text-shadow: 2px 0 0 #fff, -2px 0 0 #fff, 0 2px 0 #fff, 0 -2px 0 #fff,
    1px 1px #fff, -1px -1px 0 #fff, 1px -1px 0 #fff, -1px 1px 0 #fff;
}
.under-text {
  text-align: center;
  font-size: 3rem;
  font-family: "Oswald", sans-serif;
}
p {
  margin: 10px 0px !important;
}
.small-text {
  text-align: center;
  font-size: 1.5rem;
  font-family: "Oswald", sans-serif;
}
body {
  background: #3399ff;
}

#cityVideo {
  position: fixed;
  right: 0;
  top: 0;
  width: 100vw;
  z-index: -1;
}
</style>
