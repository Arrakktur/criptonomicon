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
            {{ city.temp}} ℃
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
            <h4 class="status__info__title">GISMETEO</h4>
            <p class="status__info__description">
              Температура: 30℃
              {{ getWeather(sel.code) }}
            </p>
            <p class="status__info__description">
              Город: {{ weather.city }}
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
        {name: 'Тула', temp: '-', code: 'Tula'},
        {name: 'Москва', temp: '-', code: 'Moscow'},
        ],
      sel: null,
      weather: {
        code: '-',
        city: '-',
        weather: '-',
        temp: '-',
        feels_like: '-',
        temp_min: '-',
        temp_max: '-',
        humidity: '-',
        grnd_level: '-',
        wind: '-',
        rain: '-',
        clouds: '-',
        dt: '-',
      }
    }
  },
  methods: {
    addCity(){
      const newCity = {
        name: this.ticket,
        temp: '-'
      };

      this.cites.push(newCity);
    },

    deleteCity(city){
      this.cites = this.cites.filter(t => t != city);
      if (this.sel == city){
        this.sel = null;
      }
    },

    getWeather(city){
      let xhr = new XMLHttpRequest();
      xhr.open('GET', this.http_request + city + this.token_openweather, false);
      xhr.send();
      if (xhr.status != 200) {
        this.error_text = "Error " + xhr.status;
      }
      else{
        return xhr.responseText;
      }
    }
  }
};

</script>

<style src="./app.css"></style>
