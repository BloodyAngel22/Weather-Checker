<script>
import axios from 'axios';

export default {
	data() {
		return {
			city: '',
			info: null,
			code: 0
		}
	},
	computed: {
		cityName() {
			return '"' + this.city + '"';
		},
		showTemp() {
			return "Temperature: " + this.info.data.main.temp + " °C"
		},
		showTempFeelsLike() {
			return "Feels Like: " + this.info.data.main.feels_like + " °C"
		},
		showHumidity() {
			return "Humidity: " + this.info.data.main.humidity + "%"
		},
		showWindSpeed() {
			return "Wind Speed: " + this.info.data.wind.speed + " m/s"
		}
	},
	methods: {
		getWeather() {
			var apiKey = "YOUR_API_KEY"; // Replace YOUR_API_KEY with your API key
			axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${apiKey}`)
				.then(res => {
					this.info = res;
					this.code = 200;
				})
				.catch(err => {
					if (err.response) {
						if (err.response.status === 404) {
							this.info = null;
							this.code = 404;
						}
					}
				})
		},
	}
}
</script>

<template>
	<div class="wrapper">
		<h1>Weather Checker</h1>
		<p>Check the weather in {{ city == "" ? "your city" : cityName }}</p>
		<input type="text" placeholder="Enter your city" v-model="city"> <br>
		<button v-if="city != ''" @click="getWeather">Get Weather</button>
		<button v-else disabled>Get Weather</button>

		<div v-if="code == 404">
			<p>City Not Found</p>
		</div>
		<div v-if="info != null">
			<p>{{ showTemp }}</p>
			<p>{{ showTempFeelsLike }}</p>
			<p>{{ showHumidity }}</p>
			<p>{{ showWindSpeed }}</p>
		</div>
	</div>
</template>

<style scoped>
.wrapper {
	width: 900px;
	height: 500px;
	border-radius: 70px;
	background-color: rgb(22, 64, 64);
	text-align: center;
	padding: 50px;
	color: #fff;
}
.wrapper h1 {
	font-size: 42px;
}
.wrapper p {
	margin-top: 25px;
	font-size: 24px;
}
.wrapper input {
	margin-top: 30px;
	width: 300px;
	height: 50px;
	border: none;
	border-radius: 10px;
	padding: 10px;
	font-size: 20px;
	border-color: #fff;
}
.wrapper button {
	margin-top: 30px;
	width: 300px;
	height: 50px;
	border: none;
	border-radius: 10px;
	font-size: 20px;
	cursor: pointer;
	border-color: #fff;
}
</style>
