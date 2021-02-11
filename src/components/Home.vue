<template>
    <div class="hello">
        <b-container>
            <h1>Погода в Ростове-на-Дону</h1>
            <div class="row">
                <b-col cols="12" md="6" xl="4" v-for="day in daily"
                       :key="`${day.dt}${day.humidity}`" class="mb-5">
                    <b-card
                        v-bind:title="getDayByWeek(day.dt)"
                        v-bind:sub-title="getDateFormat(day.dt)"
                    >
                        <b-card-text>
                            <b-row align-v="center" class="mb-3">
                                <b-col cols sm="auto">
                                    <img
                                        v-bind:src="getImageUrl(day.weather[0].icon)"
                                        v-bind:alt="day.weather[0].description"
                                    >
                                </b-col>
                                <b-col cols>
                                    {{day.weather[0].description}}
                                </b-col>
                            </b-row>
                            <b-table
                                small
                                :items="getTempTable(day.temp, day.feels_like)"
                            ></b-table>
                        </b-card-text>
                    </b-card>
                </b-col>
            </div>
        </b-container>
    </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',

  data: () => ({
    current: null,
    daily: null,
    weedDays: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'],
  }),

  methods: {
    async getWeather() {
      try {
        const response = await axios.get('https://api.openweathermap.org/data/2.5/onecall?lat=47.2364&lon=39.7139'
            + '&exclude=hourly,minutely&units=metric&lang=ru&appid=54341f93f695264ff29b1f50899c5a8c');
        console.log(response);
        return response.data;
      } catch (error) {
        console.error(error);
      }
      return true;
    },
    getDateFormat(timestamp) {
      const dt = new Date(timestamp * 1000);
      return `${dt.getDate()}.${dt.getMonth()}.${dt.getFullYear()}`;
    },
    getDayByWeek(timestamp) {
      const dt = new Date(timestamp * 1000);
      return this.weedDays[dt.getDay()];
    },
    getTempTable(temp, feels) {
      return [
        { Время: 'Утро', '°С': temp.morn, '°С по ощ.': feels.morn },
        { Время: 'День', '°С': temp.day, '°С по ощ.': feels.day },
        { Время: 'Вечер', '°С': temp.eve, '°С по ощ.': feels.eve },
        { Время: 'Ночь', '°С': temp.night, '°С по ощ.': feels.night },
      ];
    },
    getImageUrl(icon) {
      return `http://openweathermap.org/img/wn/${icon}@2x.png`;
    },
  },

  async mounted() {
    const data = await this.getWeather();
    this.current = data.current;
    this.daily = data.daily;
    console.log(data.daily);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
