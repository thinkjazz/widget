<template>
  <div
  id="app"
  :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          name=""
          class="search-bar"
          placeholder="Город...."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap"  v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateHandler() }}</div>
        </div>

        <div class="clock">
          14:20
          <p class="date"></p>
          <p class="time"></p>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>

          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="weather humidity">Влажность</div>
          <div class="weather wind">Ветер</div>

          <div class="weather-icon">
            ☔🌙🌕👽👾💣🤘🤙🖖✌🤘🧟🦊 🦑🦞🦐🦀🌂🔥🌒🚀⛄
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      url_base: "https://api.openweathermap.org/data/2.5/",
      api_key: "227933158fcf503a3aefe593cedf4e73",
      query: "Moscow",
      weather: {},
      bookmarks: [],
      time: "",
      data: "",
    };
  },

  methods: {
    fetchWeather(event) {
      if (event.key == "Enter") {
        const url = `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`;
        fetch(url)
          .then((response) => {
            return response.json();
          })
          .then(this.setDoneResults);
      }
    },

    setDoneResults(datum) {
      this.weather = datum;
    },
    dateHandler() {
      let d = new Date();
      // let formatter = new Intl.DateTimeFormat("ru-RU", {
      //   weekday: "long",
      //   year: "numeric",
      //   month: "long",
      //   day: "numeric"
      // });

      let months = [
        "Январь" ,
        "Февраль" ,
        "Март" ,
        "Апрель" ,
        "Май" ,
        "Июнь" ,
        "Июль" ,
        "Август" ,
        "Сентябрь" ,
        "Октябрь" ,
        "Ноября" ,
        "Декабрь" ,
      ];
      let days = [
        "Воскресенье",
        "Понедельник",
        "Вторник",
        "Среда",
        "Четверг",
        "Пятница",
        "Суббота"

      ];
      let day = days[d.getDay()];
      console.log(day);
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${date} ${month} ${year} ${day}`;
    }
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@200;300;400;600;700;900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Source Sans Pro", sans-serif;
}

#app {
  margin: 0 auto;
  max-width: 600px;
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
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
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.15);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px 16px 16px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.15);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: white;
  font-size: 70px;
  font-weight: 300;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.1);
}
.location-box .date {
  color: white;
  font-size: 35px;
  font-weight: 200;
  text-align: center;
}
.clock {
  font-family: "Roboto";
  text-align: center;
  color: white;
  font-size: 110px;
  font-weight: 100;
}

.weather-box {
  text-align: center;
}
.weather-box .temp {
  font-family: "Roboto";
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 130px;
  font-weight: 300;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.15);
  background-color: rgba(156, 165, 252, 0.1);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(156, 165, 252, 0.1);
}
.weather-box .weather {
  color: white;
  font-size: 30px;
  font-weight: 200;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.1);
}
.weather-box .weather-icon {
  /* color: white; */
  font-size: 36px;
  /* font-weight: 600; */
  /* text-shadow: 3px 6px rgba(0, 0, 0, 0.10); */
}
</style>
