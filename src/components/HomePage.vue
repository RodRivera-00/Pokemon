<template>
	<div className="pageContainer">
		<div className="cardContainer" v-if="pokemonList">
			<PokemonCard
				v-for="pokemon in pokemonList"
				:key="pokemon.name"
				:name="pokemon.name"
				:url="pokemon.url"
			/>
		</div>
	</div>
	<div v-if="isLoading">Loading...</div>
	<button v-if="pokemonList.length < 150" class="loadButton" @click="loadMore">
		Load More
	</button>
</template>

<script lang="ts">
import axios from "axios";
import { defineComponent } from "vue";

//Local
import PokemonCard from "./PokemonCard.vue";

interface PokemonItem {
	name: string;
	url: string;
}
export default defineComponent({
	data() {
		return {
			isLoading: false,
			pokemonList: [] as PokemonItem[],
		};
	},
	components: {
		PokemonCard,
	},
	async mounted() {
		this.isLoading = true;
		const result = await axios.get(
			"https://pokeapi.co/api/v2/pokemon?limit=50"
		);
		this.pokemonList = (await result.data.results) as PokemonItem[];
		this.isLoading = false;
	},
	methods: {
		async loadMore() {
			this.isLoading = true;
			const pokemonsLeft = 150 - this.pokemonList.length;
			const limit = pokemonsLeft >= 15 ? 15 : pokemonsLeft;
			const result = await axios.get(
				`https://pokeapi.co/api/v2/pokemon?limit=${limit}&offset=${this.pokemonList.length}`
			);
			this.pokemonList = [...this.pokemonList, ...result.data.results];
			this.isLoading = false;
		},
	},
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pageContainer {
	display: flex;
	justify-content: center;
	width: 100%;
}
.cardContainer {
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
	row-gap: 50px;
	column-gap: 50px;
	justify-content: space-evenly;
	max-width: 1440px;
	width: 100%;
}
.loadButton {
	margin-top: 50px;
	width: 100px;
	height: 50px;
}
</style>
