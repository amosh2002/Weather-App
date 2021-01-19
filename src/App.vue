<template>
  <div id="app"
       :class="imageClassName">
    <main>
      <div class="search-box">
        <input
            type="text"
            class="search-bar"
            placeholder="Search..."
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
      api_key: '7dbc614d8fe5ba304db218feb212dbac',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      imageClassName: 'warmClear',
    }
  },

  methods: {
    imageClassDefiner() {
      if (typeof this.weather.main === 'undefined') {
        return this.imageClassName
      }
      if (this.weather.main.temp > 14) {
        switch (this.weather.weather[0].main) {
          case 'Clear':
            return 'warmClear'
          case 'Clouds':
            return 'warmCloudy'
          case 'Haze':
            return 'haze'
          case 'Rain':
            return 'rainy'
          case 'Fog':
            return 'fog'
          case 'Mist':
            return 'fog'
        }
      } else {
        switch (this.weather.weather[0].main) {
          case 'Clear':
            return 'coldClear'
          case 'Clouds':
            return 'coldCloudy'
          case 'Snow':
            return 'coldSnowy'
          case 'Haze':
            return 'haze'
          case 'Rain':
            return 'rainy'
          case 'Fog':
            return 'fog'
          case 'Mist':
            return 'fog'
        }
      }

    },
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
            .then(res => {
              return res.json();
            }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      this.imageClassName = this.imageClassDefiner()
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
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
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}

#app {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warmCloudy {
  background-image: url('https://s11.favim.com/orig/7/759/7591/75916/colors-clowds-sky-Favim.com-7591634.jpg');
}

#app.warmClear {
  background-image: url('https://c.tadst.com/gfx/750w/barcelona-morning-sky.jpg?1');
}

#app.rainy {
  background-image: url('https://armenpress.am/static/news/b/2020/02/1006680.jpg');
}

#app.haze {
  background-image: url('https://arc-anglerfish-washpost-prod-washpost.s3.amazonaws.com/public/FQPQPJX64AI6TA2BZQ644UXH3Y.jpg');
}

#app.fog {
  background-image: url('https://images.unsplash.com/photo-1504335089263-e69fcfef931e?ixid=MXwxMjA3fDB8MHxzZWFyY2h8MXx8Zm9nZ3klMjBza3l8ZW58MHx8MHw%3D&ixlib=rb-1.2.1&w=1000&q=80');
}

#app.coldClear {
  background-image: url('https://i.pinimg.com/originals/df/ef/8c/dfef8cd82e08673c4f13389c4906ef8b.jpg');
}

#app.coldCloudy {
  background-image: url('https://ak.picdn.net/shutterstock/videos/1031166308/thumb/1.jpg');
}

#app.coldSnowy {
  background-image: url('https://i.ytimg.com/vi/kJGjueu-s0U/maxresdefault.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px;
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
  background-color: rgba(255, 255, 255, 0.25);
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
</style>