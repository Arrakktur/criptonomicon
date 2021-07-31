<template>
<div>
  <div class="header">
    <div class="header__wrapper">
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
  <div class="cards">
    <div 
      class="card"
      v-for="city in cites"
      v-bind:key="city.name"
    >
      <div class="card__wrapper">
        <h3 class="card__title">
          {{ city.name }}
        </h3>
        <p class="card__description">
          {{ city.temp}} ℃
        </p>
        <button class="card__button">
          Удалить
        </button>
      </div>
    </div>
  </div>
  <div>
    Статус
  </div>
</div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      title: "Погода",
      ticket: "",
      cites: [
        {name: 'Тула', temp: '-'},
        {name: 'Москва', temp: '-'},
        {name: 'Белгород', temp: '-'},
        {name: 'Орел', temp: '-'}
        ],
    }
  },
  methods: {
    addCity(){
      const newCity = {
        name: 'testCity', temp: '-'
      };

      this.cites.push(newCity);
    }
  }
};

</script>

<style src="./app.css">

</style>
