<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <!-- Search bar -->
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <!-- Result -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
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
  name: "app",
  data() {
    return {
      api_key: "cf8e328a0956d95fbc6cd69269618ceb",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },

  methods: {
    // Fetch weather
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((data) => {
            return data.json();
          })
          .then(this.setResults);
      }
    },

    // Display results
    setResults(results) {
      this.weather = results;
    },

    // Set date
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;300;500;700;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Poppins", sans-serif;
  font-size: 16pt;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
  background-size: cover;
  background-position: top;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );

  .search-box {
    width: 100%;
    margin-bottom: 30px;

    @media screen and (min-width: 768px) {
      width: 50%;
      margin: 0 auto 30px;
    }

    .search-bar {
      display: block;
      width: 100%;
      padding: 15px;
      color: #313131;
      font-size: 1.1rem;
      appearance: none;
      border: none;
      outline: none;
      background: none;
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.5);
      border-radius: 0px 16px 0px 16px;
      transition: 0.4s;
    }

    .search-bar:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0px 16px 0px;
    }
  }

  .location-box {
    .location {
      color: #fff;
      font-size: 2rem;
      font-weight: 500;
      text-align: center;
      text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
    }

    .date {
      color: #fff;
      font-size: 1.2rem;
      font-weight: 300;
      font-style: italic;
      text-align: center;
    }
  }

  .weather-box {
    text-align: center;

    .temp {
      display: inline-block;
      padding: 10px 25px;
      color: #fff;
      font-size: 4.5rem;
      font-weight: 700;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.25);
      border-radius: 16px;
      margin: 30px 0px;
      box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }

    .weather {
      color: #fff;
      font-size: 2.5rem;
      font-weight: 500;
      font-style: italic;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    }
  }
}
</style>