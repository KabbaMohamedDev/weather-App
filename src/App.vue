<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'sunny' : ''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Peux-tu écrire le nom de ta ville..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
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
                api_key: 'a80f1b8e42cd5e6ed3ebabfd8027c40f',
                url_base: 'https://api.openweathermap.org/data/2.5/',
                query: '',
                weather: {}
            }
        },
        methods: {
            fetchWeather(e) {
                if (e.key == "Enter") {
                    fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
                        .then(res => {
                            return res.json();
                        }).then(this.setResults);
                }
            },
            setResults(results) {
                this.weather = results;
            },
            dateBuilder() {
                let d = new Date();
                let months = ["Janvier", "Février", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"];
                let days = ["Dimanche", "Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi"];

                let day = days[d.getDay()];
                let date = d.getDate();
                let month = months[d.getMonth()];
                let year = d.getFullYear();

                return `${day} ${date} ${month} ${year}`;
            }
        }
    }
</script>

<style>
@import '\style.css'
</style>