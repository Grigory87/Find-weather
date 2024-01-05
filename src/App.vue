<script>
import axios from 'axios';

export default {
    data() {
        return {
            city: '',
            error: '',
            info: null
        }
    },
    computed: {
        cityName() {
          return "«" + this.city + "»"
        },
        showTemp() {
          return 'Температура воздуха: ' + this.info.data.current.temp_c
        },
        showFeelsLike() {
          return 'Ощущается как: ' + this.info.data.current.feelslike_c
        },
        showWindSpeed() {
          return 'Скорость ветра: ' + this.info.data.current.wind_mph
        }
    },
    methods: {
        getWeather() {
            if(this.city.trim().length < 2) {
              this.error = 'Нужно название более 1 символа';
              return false;
            }
            this.error = '';

            axios.get(`http://api.weatherapi.com/v1/current.json?key=869a1d46cde94ddc946124108240501&q=${this.city}`)
                .then(res => (this.info = res))
        }
    }
  }        
</script>


<template>
    <div class="wrapper">
        <h1>Погодное приложение</h1>
        <p>Узнать погоду в {{ city == '' ? ' вашем городе' : cityName}}</p>
        <input type="text" v-model="city" @input="info=null" placeholder="Ваш город">
        <button v-if="city != ''" @click="getWeather()">Запросить</button>
        <button disabled v-else>Введите название города</button>

        <p class="error">{{ error }}</p>

        <div class="info" v-if="info != null">
          <p>{{ showTemp }} °C</p>
          <p>{{ showFeelsLike }} °C</p>
          <p>{{ showWindSpeed }} м/с</p>
        </div>
        
    </div>
</template>


<style scoped>
.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 50px;
    padding: 20px;
    background: rgb(3,227,202);
    background: linear-gradient(0deg, rgba(3,227,202,0.9976365546218487) 3%, rgba(79,228,119,1) 43%, rgba(151,162,244,1) 81%);
    text-align: center;
    color:rgb(20, 24, 24);
}

.wrapper h1 {
    margin-top: 5%;
    font-size: 40px;
}

.wrapper input {
    margin-top: 5%;
    background: transparent;
    border: 0;
    border-bottom: 2px solid royalblue;
    color: black;
    font-size: 14px;
    padding: 5px 8px;
    outline: none;
}

.wrapper input:focus {
    border-bottom-color: rgb(94, 0, 128);
}

.wrapper button {
    background: rgb(40, 156, 88);
    color: black;
    border-radius: 5px;
    border: 1px solid rgb(180, 182, 233);
    padding: 5px 15px;
    margin-left: 20 px;
    cursor: pointer;
    transition: transform 250ms ease;
}
    
.wrapper button:hover {
    transform: scale(1.1) translateY(-1px);
}

.wrapper button:disabled {
    cursor: not-allowed;
    background: rgb(191, 171, 156);
}

.error {
  color: brown;
}

.info {
  margin-top: 20px;
  font-size: 20px;
}

.info p {
  margin-top: 10px;
}
</style>
