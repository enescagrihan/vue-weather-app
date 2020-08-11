<template>
  <div id="app">
    <div class="search-input">
      <input type="text" v-model="query" @keypress.enter="getWeatherInfo()" placeholder="Search for a city..." />
    </div>

    <div class="city">
      <h1 v-if="weather.name">{{ weather.name }}</h1>
      <h1 v-else>---, --</h1>
    </div>

    <div class="date">
      <p>{{ dateBuild() }}</p>
    </div>

    <div class="degree">
      <h1 v-if="weather.weather">{{ Math.round(weather.main.temp) }} °C</h1>
      <h2 v-else>-- °C</h2>
    </div>

    <div class="weather">
      <h2 v-if="weather.weather"> {{ weather.weather[0].main }} </h2>
      <h2 v-else>-----</h2>
    </div>

    <div class="hi-low">
      <h3 v-if="weather.weather">{{ weather.main.temp_max }}°C/{{ weather.main.temp_min }}°C</h3>
      <h3 v-else>--°C/--°C</h3>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data () {
    return {
      apiKey : '105883e01debe887198f5e57b066bdfd',
      query : '',
      weather : {}
    }
  },
  methods : {
    getWeatherInfo() {
      fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=${this.apiKey}`)
      .then(res => {
        return res.json()
      }) .then(this.setResults)
      this.query = ''
    },
    setResults(results) {
      this.weather = results
      console.log(results);
    },
    dateBuild() {
      const d = new Date()
      const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
      const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
      const date = d.getDate()
      const day = days[d.getDay()]
      const month = months[d.getMonth()]
      const year = d.getFullYear()

      return `${day} ${date} ${month} ${year}`
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
  background-image: url("./assets/İstanbul.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: top;
  background-attachment: fixed;
  min-height: 100vh;
}

#app {
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.05),
    rgba(0, 0, 0, 0.45)
  );
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.search-input{
  width: 100%;
  padding: 50px;
  text-align: center;
}

.search-input input {
  width: 25%;
  min-width: 205px;
  padding: 15px;
  background-color: rgba(0, 0, 0, 0.10);
  border: 1px solid #fff;
  border-radius: 10px;
  color: #fff;
  font-size: 18px;
}

.city {
  color: #fff;
  font-size: 17px;
}

.date {
  margin: 15px 0 70px 0;
  color: #fff;
}

.degree {
  font-size: 45px;
  color: #fff;
  margin-bottom: 65px;
}

.weather {
  font-size: 20px;
  color: #fff;
  margin-bottom: 20px;
}

.hi-low {
  font-size: 18px;
  color: #fff;
}


</style>
