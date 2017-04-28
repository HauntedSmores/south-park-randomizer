<template>
  <div id="app">
	  <div class="container">
		  <div class="row">
			<div class="col-md-12">
				<h1>South Park Randomizer</h1>
				<p>{{ randomNumber }}</p>
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
			const key = 'api_key=11067bbd9f322ba00d3797f02bca8c91f4f41154';
			var http = axios.create({baseURL: 'http://api-public.guidebox.com/v2/'});

			http.get('shows/405/episodes?season=1&' + key)
			.then(response => {
				console.log(response);
				this.data = response.data;
			})
			.catch(function (error) {
				console.log(error);
			});
		},
		data () {
			return {
				data: {},
				randomNumber: "No random number."

			}
		},
		methods: {
			generate: function() {
				this.randomNumber = Math.floor(Math.random() * (13 - 1)) + 1;
				console.log(this.randomNumber);
			}
		}
	}
</script>
<style src="./assets/scss/app.scss" lang="scss"></style>
