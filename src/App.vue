<template>
<div class="container">
  <div class="header">
    <div class="header__wrapper">
      <span 
        v-if="error_text"
        class="errorText"
      >
        {{ error_text }}
      </span>
      <h1 class="header__title">{{ title }}</h1>
      <label class="header__description">
        Введите город:
      </label>
      <input 
        v-model="ticket"
        v-on:keydown.enter="addCity"
        type="text"
        class="header__input"
        placeholder="Введите город"
      >
      <button
        class="header__button"
        @click="addCity"
      >
        Добавить
      </button>
    </div>
  </div>
  <template v-if="cites.length > 0">
    <div class="cards">
      <div 
        v-for="city in cites"
        v-bind:key="city.name"
        :class="{
            'card__active': sel === city
          }"
        @click="sel = city"
        class="card"
      >
        <div class="card__wrapper">
          <h3 class="card__title">
            {{ city.name }}
          </h3>
          <p class="card__description">
            - ℃
          </p>
          <button 
            class="card__button"
            @click.stop="deleteCity(city)"
          >
            Удалить
          </button>
        </div>
      </div>
    </div>
    <template
      v-if="sel"
    >
      <div class="status">
        <h3 class="status__title">
          Название города: {{ sel.name }}
        </h3>
        <span
          @click="sel = null"
          class="status__close"
        >
          Закрыть
        </span>
        <div class="status__infoWrap">
          <div class="status__info">
            {{ getWeather(sel.name) }}
            <h4 class="status__info__title">OpenWeather</h4>
            <p class="status__info__description">
              Город: {{ weather.name }} ({{ weather.coord.lon }}, {{ weather.coord.lat }})<br>
              Температура: {{ weather.main.temp }} ℃<br>

            </p>
          </div>
        </div>
      </div>
    </template>
  </template>
</div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      token_openweather: '&appid=72b5518d8b2715b80d1b9ca1d364197f',
      http_request: 'http://api.openweathermap.org/data/2.5/weather?lang=ru&q=',

      title: 'Погода',
      ticket: '',
      error_text: '',
      cites: [
        {name: 'Тула'},
        {name: 'Москва'},
        ],
      sel: null,
      weather: {
        coord: {
          lon: '-',
          lat: '-',
        },
        weather: {
          main: '-',
          description: '-',
          icon: '-',
        },
        main: {
          temp: '-',
          feels_like: '-',
          pressure: '-',
          humidity: '-',
          temp_min: '-',
          temp_max: '-',
          sea_level: '-',
          grnd_level: '-',
        },
        wind: {
          speed: '-',
          deg: '-',
          gust: '-',
        },
        clouds: '-',
        rain: '-',
        snow: '-',
        dt: '-',
        sys: {
          sunrise: '-',
          sunset: '-',
        },
        name: '-',
      }
    }
  },

  methods: {
    addCity(){
      const newCity = {
        name: this.ticket,
      };

      this.cites.push(newCity);
      this.ticket = "";
    },

    getWeather(city){
      fetch(this.http_request + city + this.token_openweather)
        .then(response => response.json())
        .then(data => {
          this.weather.name = data.name;
          this.weather.coord.lon = data.coord.lon;
          this.weather.coord.lat = data.coord.lat;
          this.weather.weather.main = data.weather.main;
          this.weather.weather.description = data.weather.description;
          this.weather.weather.icon = data.weather.icon;
          this.weather.main.temp = Math.round(data.main.temp - 273.15);
          this.weather.main.feels_like = Math.round(data.main.feels_like - 273.15);
          this.weather.main.pressure = data.main.pressure;
        });
    },

    deleteCity(city){
      this.cites = this.cites.filter(t => t != city);
      if (this.sel == city){
        this.sel = null;
      }
    },
  }
};

</script>

<style src="./app.css"></style>