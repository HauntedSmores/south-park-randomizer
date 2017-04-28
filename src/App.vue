<template>
  <div id="app">
	  <div class="container">
		  <div class="row">
			<div class="col-md-12">
				<h1>South Park Randomizer</h1>
				<p>{{ randomEpisode }}</p>
				<button v-on:click="generate()">Generate Episode</button>
			</div>
		  </div>
	  </div>
  </div>
</template>

<script>
import * as axios from 'axios';
export default {
		name: 'app',
		created () {
			var http = axios.create({baseURL: 'http://api-public.guidebox.com/v2/'});

			http.get('shows/405/seasons?' + this.key)
			.then(response => {
				console.log(response);
				this.total_seasons = response.data.total_results;
			})
			.catch(function (error) {
				console.log(error);
			});
		},
		data () {
			return {
				data: {},
				key: 'api_key=11067bbd9f322ba00d3797f02bca8c91f4f41154',
				total_seasons: "",
				total_episodes: "",
				season: "",
				episode: "",
				randomEpisode: "No random number."

			}
		},
		methods: {
			generate: function() {
				var http = axios.create({baseURL: 'http://api-public.guidebox.com/v2/'});
				let season = Math.floor(Math.random() * (this.total_seasons - 1)) + 1;
				let episode;
				http.get('shows/405/episodes?season=' + season + '&' + this.key)
				.then(response => {
					this.total_episodes = response.data.total_results;
					console.log(this.total_episodes);
					episode = Math.floor(Math.random() * (this.total_episodes - 1)) + 1;
					this.randomEpisode = "Season " + season + " Episode " + episode;
					this.season = season;
					this.episode = episode;
				})
				.catch(function (error) {
					console.log(error);
				});
			},
			getEpisode: function() {
			}
		}
	}
</script>
<style src="./assets/scss/app.scss" lang="scss"></style>
