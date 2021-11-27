<template>
<div>
	<div class="container" v-if="typeof weather.main != 'undefined'">
		<div class="weather-side">
			<div class="weather-gradient"></div>
			{{weather.list}}
			<div class="date-container">
				<h2 class="date-dayname">{{ getDate.weekday }}</h2>
				<span class="date-day">{{ getDate.date }}</span>
				<span class="location">
					<span class="bookmark-add" @click="addToBookmarks()" title="add to bookmark"><i class="fas fa-bookmark" /></span>
					{{ weather.name }}, {{ weather.sys.country }}
				</span>
			</div>
			

			<div class="weather-container">
				<h1 class="weather-icon">
				{{dt_txt}}
				</h1>
				<h1 class="weather-temp">{{ Math.round(weather.main.temp) }}°C</h1>
				<h3 class="weather-desc">{{ weather.weather[0].description }}</h3>
			</div>
		</div>

		<div class="info-side">
			<div class="today-info-container">
				<div class="today-info">
					<div class="humidity">
						<span class="title">Влажность</span><span class="value">{{ weather.main.humidity }} %</span>
						<div class="clear"></div>
					</div>
					
					<div class="wind">
						<span class="title">Ветер</span><span class="value">{{ weather.wind.speed }} km/h</span>
						<div class="clear"></div>
					</div>
				</div>
			</div>
			

			<div class="location-container">
				<form @submit.prevent="fetchWeather()" accept-charset="utf-8">
					<input class="location-input" type="text" v-model="query" placholder="Введите своё местоположение" />
					<button type="submit" class="location-button">
						<i class="location-icon fas fa-map-marker-alt"></i> <span>Сменить местоположение</span>
					</button>
				</form>
			</div>
		</div>
		
		<div v-if="bookmarks.length" class="bookmarks">
			<div class="bookmarks-button"><span /></div>
			<h3>Заметки:</h3>
			<ul class="bookmarks-list">
				<li
					@click="
						query = bookmark;
						fetchWeather;
					"
					v-for="(bookmark, index) in bookmarks"
					:key="index"
				>
					{{ bookmark }}
				</li>
			</ul>
		</div>
		<div>
			<div>
				<table>
					<tr>
						<td>
							<h5  @click="setDay(1)" class="days" >{{ getDate.day_1 }}</h5>
						</td>
						<td>
							<h5 @click="setDay(2)" class="days">{{ getDate.day_2 }}</h5>
						</td>
						<td>
							<h5  @click="setDay(3)" class="days">{{ getDate.day_3 }}</h5>
						</td>
						<td>
							<h5  @click="setDay(4)" class="days">{{ getDate.day_4 }}</h5>
						</td>
						<td>
							<h5  @click="setDay(5)" class="days">{{ getDate.day_5 }}</h5>
						</td>
					</tr>
				</table>
			</div>
		</div>
	</div>
</div>
</template>

<script>
import axios from "axios";

export default {
	name: "App",
	data: () => ({
		api_url: "https://api.openweathermap.org/data/2.5/",
		api_key: "f423841ca31681353258a0e7361398c3",
		query: "Москва",
		weather: {},
		bookmarks: [],
		day: 1
	}),
	methods: {
		setDay(day) {
			//this.weekday[d.getDay() + 1]

			this.day = day;
			
			axios
			.get(`${this.api_url}weather?q=${this.query}&units=metric&appid=${this.api_key}&lang=ru&cnt=${this.day}`)
			.then((response) => (this.weather = response.data))
			.catch((error) => console.log(error));

		
		},
	
		addToBookmarks() {
			console.log(this.bookmarks.indexOf(this.query));
			if (this.bookmarks.indexOf(this.query) !== -1) {
				return;
			}
			this.bookmarks.push(this.query);
		},
	},
	computed: {
		getDate: function () {
			const d = new Date();
			const monthNames = [
				"Январь",
				"Февраль",
				"Март",
				"Апрель",
				"Май",
				"Июнь",
				"Июль",
				"Август",
				"Сентябрь",
				"Октябрь",
				"Ноябрь",
				"Декабрь",
			];
			const month = d.getMonth();
			const day_1 = `${d.getDate() +1}, ${monthNames[month]} ${d.getFullYear()}`;
			const day_2 = `${d.getDate() +2}, ${monthNames[month]} ${d.getFullYear()}`;
			const day_3 = `${d.getDate() +3}, ${monthNames[month]} ${d.getFullYear()}`;
			const day_4 = `${d.getDate() +4}, ${monthNames[month]} ${d.getFullYear()}`;
			const day_5 = `${d.getDate() +5}, ${monthNames[month]} ${d.getFullYear()}`;
			const weekday = ["Воскресенье", "Понедельник", "Вторник", "Среда", "Четверг", "Пятница", "Суббота"][d.getDay()];
			console.log(d.getDay());
			const date = `${d.getDate()}, ${monthNames[month]} ${d.getFullYear()}`;
			return { date, weekday, day_1,day_2, day_3, day_4, day_5  };
		},
	},
	mounted() {
		this.setDay();
	},
	day_2(){
			axios
				.get(`${this.api_url}weather?q=${this.query}&units=metric&appid=${this.api_key}&lang=ru&cnt=2`)
				.then((response) => (this.weather = response.data))
				.catch((error) => console.log(error));
	}
};
</script>

<style lang="scss">
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}
@import url("https://fonts.googleapis.com/css?family=Montserrat:400,700,900&display=swap");

:root {
	--gradient: linear-gradient(135deg, #99ff99 10%, #2fd829 90%);
}

* {
	-webkit-box-sizing: border-box;
	box-sizing: border-box;
	line-height: 1.25em;
}

.clear {
	clear: both;
}

body {
	margin: 0;
	width: 100%;
	height: 100vh;
	font-family: "Montserrat", sans-serif;
	background-color: #343d4b;
	display: -webkit-box;
	display: -ms-flexbox;
	display: flex;
	-webkit-box-align: center;
	-ms-flex-align: center;
	align-items: center;
	-webkit-box-pack: center;
	-ms-flex-pack: center;
	justify-content: center;
}

.container {
	border-radius: 25px;
	-webkit-box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
	box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
	background-color: #222831;
	color: #ffffff;
	height: 400px;
}

.weather-side {
	position: relative;
	height: 100%;
	border-radius: 25px;
	background-image: url("https://images.unsplash.com/photo-1559963110-71b394e7494d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80");
	width: 300px;
	-webkit-box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
	box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
	float: left;
	-webkit-transform: scale(1.1) perspective(1500px) rotateY(10deg);
	transform: scale(1.1) perspective(1500px) rotateY(10deg);
	z-index: 2;
}

.weather-gradient {
	position: absolute;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	background-image: var(--gradient);
	border-radius: 25px;
	opacity: 0.8;
}

.date-container {
	position: absolute;
	top: 25px;
	left: 25px;
}

.date-dayname {
	margin: 0;
}

.date-day {
	display: block;
}
.days{
	display: block;
	padding: 25px;
	background:linear-gradient(135deg, #99ff99 10%, #2fd829 90%);
	border-radius: 25px;
	top: 25px;
}

.location {
	display: inline-block;
	margin-top: 10px;
}

.location-icon {
	display: inline-block;
	font-size: 1em;
	margin-right: 7px;
}

.weather-container {
	position: absolute;
	bottom: 25px;
	left: 25px;
}

.weather-icon {
	font-size: 60px;
}

.weather-temp {
	margin: 0;
	font-weight: 700;
	font-size: 4em;
}

.weather-desc {
	margin: 0;
}

.info-side {
	position: relative;
	float: left;
	height: 100%;
	padding-top: 25px;
}

.info-logo {
	display: block;
	margin: 0 auto;
	margin-top: -140px;
	padding-bottom: 20px;
}

.today-info {
	padding: 15px;
	margin: 0 25px 25px 25px;
	border-radius: 10px;
}

.today-info > div:not(:last-child) {
	margin: 0 0 10px 0;
}

.today-info > div .title {
	float: left;
	font-weight: 700;
}

.today-info > div .value {
	float: right;
}

.week-list {
	list-style-type: none;
	padding: 0;
	margin: 10px 35px;
	-webkit-box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
	box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
	border-radius: 10px;
}

.week-list > li {
	float: left;
	padding: 15px;
	transition: 200ms ease;
	border-radius: 10px;
}

.week-list > li.active {
	background: #fff;
	color: #222831;
	border-radius: 10px;
}

.week-list > li .day-name {
	display: block;
	margin: 10px 0 0 0;
	text-align: center;
}

.week-list > li .day-icon {
	display: block;
	height: 30px;
	width: auto;
	margin: 0 auto;
}

.week-list > li .day-temp {
	display: block;
	text-align: center;
	margin: 10px 0 0 0;
	font-weight: 700;
}

.location-container {
	padding: 25px 35px;
}

.location-button {
	outline: none;
	width: 100%;
	-webkit-box-sizing: border-box;
	box-sizing: border-box;
	border: none;
	border-radius: 25px;
	padding: 10px;
	font-family: "Montserrat", sans-serif;
	background-image: var(--gradient);
	color: #ffffff;
	font-weight: 700;
	-webkit-box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
	box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
	cursor: pointer;
	-webkit-transition: -webkit-transform 200ms ease;
	transition: -webkit-transform 200ms ease;
	-o-transition: transform 200ms ease;
	transition: transform 200ms ease;
	transition: transform 200ms ease, -webkit-transform 200ms ease;
}

.location-input {
	outline: none;
	width: 100%;
	-webkit-box-sizing: border-box;
	box-sizing: border-box;
	border: none;
	border-radius: 25px;
	padding: 10px 20px;
	font-family: "Montserrat", sans-serif;
	color: #ffffff;
	background: none;
	font-weight: 700;
	border: 2px solid rgba(#99ff99, 0.5);
	margin-bottom: 15px;
}

.location-input:focus {
	border-color: rgba(#99ff99, 0.8);
}

.location-button:hover {
	-webkit-transform: scale(0.95);
	-ms-transform: scale(0.95);
	transform: scale(0.95);
}

.location-button .feather {
	height: 1em;
	width: auto;
	margin-right: 5px;
}

.bookmarks {
	position: absolute;
	right: 0;
	top: 0;
	width: 300px;
	background: #222831;
	text-align: center;
}
.bookmarks-list {
	list-style: none;
	padding: 0;
	li {
		margin-bottom: 7px;
		cursor: pointer;
		&:hover {
			text-decoration: underline;
		}
	}
}
.bookmark-add {
	cursor: pointer;
	&:hover {
		color: #323232;
	}
}
</style>
