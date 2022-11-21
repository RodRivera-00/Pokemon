<template>
	<div class="pokemonCard" v-if="isLoading">Loading...</div>
	<div class="pokemonCard" v-if="!isLoading">
		<img class="cardImage" :src="pokemon.image" />
		<h4 class="pokemonName">{{ pokemon.name.capitalize() }}</h4>
		<div class="statsContainer">
			<div class="stat">
				<span class="header">Height</span
				><span>{{ pokemon.height }}&nbsp;inches</span>
			</div>
			<div class="stat">
				<span class="header">Weight</span>
				<span>{{ pokemon.weight }}&nbsp;g</span>
			</div>
		</div>
	</div>
</template>
<script>
import axios from "axios";
import { defineComponent } from "vue";

export default defineComponent({
	data() {
		return {
			isLoading: false,
			pokemon: {
				name: "",
				height: 0,
				weight: 0,
				image: "",
			},
		};
	},
	async mounted() {
		this.isLoading = true;
		const result = await axios.get(this.url);
		this.pokemon = {
			name: result.data.name,
			height: result.data.height,
			weight: result.data.weight,
			image: result.data.sprites.other["official-artwork"].front_default,
		};
		this.isLoading = false;
	},
	props: {
		name: String,
		url: String,
	},
});

Object.defineProperty(String.prototype, "capitalize", {
	value: function () {
		return this.charAt(0).toUpperCase() + this.slice(1);
	},
	enumerable: false,
});
</script>
<style scoped>
.pokemonCard {
	border: 1px solid black;
	width: 200px;
	min-width: 200px;
	height: 300px;
	border-radius: 10px;
}
.cardImage {
	width: 80%;
	padding: 20px;
}
.pokemonName {
	margin: 0px;
}
.statsContainer {
	margin-top: 10px;
	padding-left: 20px;
	padding-right: 20px;
	display: flex;
	justify-content: space-between;
}
.stat {
	display: flex;
	flex-direction: column;
}
.stat > .header {
	font-size: 12px;
	font-weight: 700;
}
</style>
