<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <section class="today">
        <div class="search-box">
          <input 
            type="text" 
            class="search-bar" 
            placeholder="Search..."
            v-model="query"
            @keyup.enter="fetchWeather"
          />
        </div>

        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
            <div class="date">{{ dateBuilder() }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">
              {{ Math.round(weather.main.temp) }}°c
              </div>
            <div class="weather">
              {{ weather.weather[0].main }}
            </div>
          </div>
        </div>
      </section>
      <section v-if="view" class="all-days  weather-box">
          <ul class="day">
            <li class="day2">{{countDays()}}</li>
            <li class="temp2">{{Math.round(weatherDays[1].temp.day)}}°c</li>
            <li>{{weatherDays[1].weather[0].main}}</li>
          </ul>
          <ul class="day">
            <li class="day2">{{countDays()}}</li>
            <li class="temp2">{{Math.round(weatherDays[2].temp.day)}}°c</li>
            <li>{{weatherDays[2].weather[0].main}}</li>
          </ul>
          <ul class="day">
            <li class="day2">{{countDays()}}</li>
            <li class="temp2">{{Math.round(weatherDays[3].temp.day)}}°c</li>
            <li>{{weatherDays[3].weather[0].main}}</li>
          </ul>
          <ul class="day">
            <li class="day2">{{countDays()}}</li>
            <li class="temp2">{{Math.round(weatherDays[4].temp.day)}}°c</li>
            <li>{{weatherDays[4].weather[0].main}}</li>
          </ul>
          <ul class="day">
            <li class="day2">{{countDays()}}</li>
            <li class="temp2">{{Math.round(weatherDays[5].temp.day)}}°c</li>
            <li>{{weatherDays[5].weather[0].main}}</li>
          </ul>
          <ul class="day">
            <li class="day2">{{countDays()}}</li>
            <li class="temp2">{{Math.round(weatherDays[6].temp.day)}}°c</li>
            <li>{{weatherDays[6].weather[0].main}}</li>
          </ul>
          <ul class="day">
            <li class="day2">{{countDays()}}</li>
            <li class="temp2">{{Math.round(weatherDays[7].temp.day)}}°c</li>
            <li>{{weatherDays[7].weather[0].main}}</li>
          </ul>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  
  data () {
    return {
      api_key: '02400b3ec5b08d0c3d23a6f587e55e1f',
      api_key2: '9849b337b30462202c3323577680b05a', 
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      day: '',
      date: '',
      month: '',
      year: '',
      longitude: '',
      latitude: '',
      weatherDays: {},
      forecastDays: [],
      view: false,
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&lang=pt_br&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      let longitude= results.coord.lon
      let latitude= results.coord.lat
      this.weather = results;

      this.fetchSecondApi(latitude, longitude);

    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      this.day = days[d.getDay()];
      this.date = d.getDate();
      this.month = months[d.getMonth()];
      this.year = d.getFullYear();

      return `${this.day} ${this.date} ${this.month} ${this.year}`;
    },
    fetchSecondApi (latitude, longitude) {
      fetch(`https://api.openweathermap.org/data/2.5/onecall?lat=${latitude}&lon=${longitude}&exclude=minutely,hourly,alerts&units=metric&appid=${this.api_key}`)
      .then(res => res.json())
      .then(this.setAllResults)
      .catch(error => {
        return error
      })
    },
    setAllResults (results) {
      this.weatherDays = results.daily
      this.view = true
    },
    countDays() {
      this.date += 1
      return `${this.date} ${this.month} ${this.year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.today {
  height: 30%;
}

.search-box {
  width: 100%;
  margin-bottom: 1.2rem;
}

.search-box .search-bar {
  display: block;
  width: 50%;
  height: 3rem;
  text-align: center;
  margin: auto;
  
  color: #313131;
  font-size: 20px;

  appearance: none;
  border:none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.all-days {
  display: flex;
  flex-flow: row wrap;
  margin: 2rem;
  height: 50%;
  color: #FFF;
  font-size: 24px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

ul {
  width: 20%;
  margin: auto;
  padding-bottom: 2rem;
}

li {
  list-style: none;
}

.temp2 {
  display: inline-block;
  padding: 1rem;
  color: rgb(255, 255, 255);
  font-size: 3rem;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.day2 {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
</style>
