<template>
  <div id="app">
	  <div class="container">
		  <div class="row header">
			<div class="col-md-12">
				<h1>South Park Randomizer</h1>
				<button v-on:click="generate()">Generate Episode</button>
			</div>
		  </div>
		  <div class="row" v-if="!randomEpisode">
			  <div class="col-md-12">
				  <svg width="45" height="45" viewBox="0 0 45 45" xmlns="http://www.w3.org/2000/svg" stroke="#fff">
					  <g fill="none" fill-rule="evenodd" transform="translate(1 1)" stroke-width="2">
						  <circle cx="22" cy="22" r="6" stroke-opacity="0">
							  <animate attributeName="r"
							  begin="1.5s" dur="3s"
							  values="6;22"
							  calcMode="linear"
							  repeatCount="indefinite" />
							  <animate attributeName="stroke-opacity"
							  begin="1.5s" dur="3s"
							  values="1;0" calcMode="linear"
							  repeatCount="indefinite" />
							  <animate attributeName="stroke-width"
							  begin="1.5s" dur="3s"
							  values="2;0" calcMode="linear"
							  repeatCount="indefinite" />
						  </circle>
						  <circle cx="22" cy="22" r="6" stroke-opacity="0">
							  <animate attributeName="r"
							  begin="3s" dur="3s"
							  values="6;22"
							  calcMode="linear"
							  repeatCount="indefinite" />
							  <animate attributeName="stroke-opacity"
							  begin="3s" dur="3s"
							  values="1;0" calcMode="linear"
							  repeatCount="indefinite" />
							  <animate attributeName="stroke-width"
							  begin="3s" dur="3s"
							  values="2;0" calcMode="linear"
							  repeatCount="indefinite" />
						  </circle>
						  <circle cx="22" cy="22" r="8">
							  <animate attributeName="r"
							  begin="0s" dur="1.5s"
							  values="6;1;2;3;4;5;6"
							  calcMode="linear"
							  repeatCount="indefinite" />
						  </circle>
					  </g>
				  </svg>
			  </div>
		  </div>
		  <div class="row" v-if="randomEpisode">
			<div class="col-md-8">
				<img v-bind:src="randomEpisode.thumbnail_608x342"/>
			</div>
			<div class="col-md-4">
				<h1>{{ randomEpisode.title }}</h1>
				<h2>{{ episodeString }}</h2>
				<p>{{ randomEpisode.overview }}</p>
				<a v-for="link in randomEpisode.subscription_web_sources" v-bind:href="link.link">{{ link.display_name }}</a>
				<template>
					<a v-for="link in randomEpisode.free_web_sources" v-bind:href="link.link">{{ link.display_name }}</a>
				</template>
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
				this.total_seasons = response.data.total_results;
				this.generate();
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
				episodeString: "",
				randomEpisode: {}

			}
		},
		methods: {
			generate: function() {
				var http = axios.create({baseURL: 'http://api-public.guidebox.com/v2/'});
				let season = Math.floor(Math.random() * (this.total_seasons - 1)) + 1;
				let episode;
				this.randomEpisode = null;
				http.get('shows/405/episodes?season=' + season + '&' + 'reverse_ordering=true&include_links=true&' + this.key)
				.then(response => {
					this.total_episodes = response.data.total_results;
					episode = Math.floor(Math.random() * (this.total_episodes - 1)) + 1;
					this.randomEpisode = response.data.results[episode - 1];
					this.episodeString = "Season " + season + " Episode " + episode;
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
