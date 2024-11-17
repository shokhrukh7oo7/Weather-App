<script>
import axios from 'axios';
export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного сивола!"
        return false;
      }
      this.error = "";

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=f3076b70915d47e49db1ee1417b71a08`)
        .then(res => (this.info = res.data))
    },
  },
  computed: {
    cityName() {
      return "«" + this.city + "»"
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max
    }
  }
}
</script>

<template>
  <div class="container">
    <div class="wrapper">
      <h1>Погодное приложение</h1>
      <p>Узнать погоду в {{ city == "" ? "вашем городе" : cityName }}</p>
      <input type="text" v-model="city" @keydown.enter="getWeather" placeholder="Введите город">
      <button v-if="city != ''" @click="getWeather()">Получить город</button>
      <button disabled v-else>Введите название города</button>
      <p class="error">{{ error }}</p>

      <div class="temp-info" v-if="info != null">
        <p>{{ showTemp }}</p>
        <p>{{ showFeelsLike }}</p>
        <p>{{ showMinTemp }}</p>
        <p>{{ showMaxTemp }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.temp-info p {
  color: goldenrod;
}

.wrapper {
  width: 100%;
  max-width: 700px;
  height: 500px;
  padding: 30px;
  border-radius: 18px;
  background: #1f0f24;
  color: #fff;
  text-align: center;
  -webkit-box-shadow: 0px 4px 20px 2px rgba(255, 0, 0, 0.4);
  -moz-box-shadow: 0px 4px 20px 2px rgba(255, 0, 0, 0.4);
  box-shadow: 0px 4px 20px 2px rgba(255, 0, 0, 0.4);
  margin: 0 auto;
}

.wrapper h1 {
  font-weight: 600;
  font-size: clamp(24px, 4vw, 36px);
  margin-bottom: 10px;
}

.wrapper p {
  font-weight: 400;
  font-size: clamp(16px, 2vw, 20px);
  margin-bottom: 10px;
}

.wrapper input {
  background: transparent;
  border: none;
  border-bottom: 2px solid #6e2d7d;
  height: 40px;
  width: 100%;
  max-width: 300px;
  color: #fff;
  outline: none;
  font-size: clamp(12px, 1vw, 16px);
}

.wrapper input:focus {
  border-bottom: 2px solid #e46aff;
}

.wrapper button {
  background-color: lightcoral;
  color: #fff;
  padding: 12px 15px;
  border-radius: 8px;
  border: 2px solid lightcoral;
  margin-left: 20px;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  background-color: rgb(255, 104, 104);
  transform: scale(1.03);
}

.error {
  color: red;
  margin: 5px 0;
  padding: 0;
  font-size: clamp(12px, 1vw, 14px);
}

@media (max-width: 576px) {
  .wrapper button {
    margin: 20px;
  }
}
</style>
