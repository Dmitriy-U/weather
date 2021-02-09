<template>
    <div class="hello">
        <b-container>
            <h1>Погода в Ростове-на-Дону</h1>
            <ul>
                <li></li>
            </ul>
        </b-container>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'Home',
        data: () => ({
            current: null,
            dayly: null,
        }),
        methods: {
            async getWeather() {
                try {
                    const response = await axios.get('https://api.openweathermap.org/data/2.5/onecall?lat=47.2364&lon=39.7139&exclude=hourly,minutely&appid=54341f93f695264ff29b1f50899c5a8c');
                    console.log(response);
                    return response.data;
                } catch (error) {
                    console.error(error);
                }
            }
        },

        async mounted() {
            const data = await this.getWeather();
            this.current = data.current;
            this.dayly = data.daily;
        },
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
