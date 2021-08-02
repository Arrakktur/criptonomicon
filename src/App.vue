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
        @click="selectCity(city)"
        class="card"
      >
        <div class="card__wrapper">
          <h3 class="card__title">
            {{ city.name }}
          </h3>
          <p class="card__description">
            {{ city.temp }} ℃
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
              Город: {{ weather.name }} ({{ weather.coord.lon }}, {{ weather.coord.lat }}), {{ weather.weather.description }}<br>
              Температура: {{ weather.main.temp }} ℃<br>
              Ощущается как: {{ weather.main.feels_like }} ℃<br>
              Атмосферное давление: {{ weather.main.pressure }}<br>
              Влажность: {{ weather.main.humidity }}<br>
              Скорость ветра: {{ weather.wind.speed }}
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
        {name: 'Тула', temp: '-'},
        {name: 'Москва', temp: '-'},
        ],
      sel: null,
      weather: {
        coord: {
          lon: '-',
          lat: '-',
        },
        weather: {
          description: '-',
        },
        main: {
          temp: '-',
          feels_like: '-',
          pressure: '-',
          humidity: '-',
        },
        wind: {
          speed: '-',
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
          this.weather.main.temp = Math.round(data.main.temp - 273.15);
          this.weather.main.feels_like = Math.round(data.main.feels_like - 273.15);
          this.weather.main.pressure = data.main.pressure;
          this.weather.main.humidity = data.main.humidity;
          this.weather.wind.speed = data.wind.speed;
          this.weather.weather.description = data.weather[0].description;
        });
    },

    selectCity(city){
      this.sel = city;
      this.getWeather(city.name);
    },

    deleteCity(city){
      this.cites = this.cites.filter(t => t != city);
      if (this.sel == city){
        this.sel = null;
      }
    },
  }
};

</script><style src="./app.css"></style>