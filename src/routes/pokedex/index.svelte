<script>
	import { pokemon } from '../../stores/pokestore.js';
	import PokemanCard from '../../components/pokemanCard.svelte';

	// console.log('pokemon = ', $pokemon);

	// const GoogleImages = require('google-images');

	// const client = new GoogleImages('CSE ID', 'API KEY');
	//
	let searchTerm = '';
	let filteredPokemon = [];

	$: {
		console.log(searchTerm);
		if (searchTerm) {
			filteredPokemon = $pokemon.filter((pokeman) =>
				pokeman.name.toLowerCase().includes(searchTerm.toLowerCase())
			);
		} else {
			filteredPokemon = [...$pokemon];
		}
	}

	// client.search('Steve Angello')
</script>

<h1 class="text-4xl text-center my-8 uppercase">svelte pokedex</h1>
<!-- <div class="py-4 grid gap-4 md:grid-cols-2 grid-cols-1"> -->

<div class="searchBarWrap">
	<input
		class="input w-full rounded-md text-lg p-4 border-2 border-gray-200"
		type="text"
		placeholder="search pokemon"
		bind:value={searchTerm}
	/>
</div>
<div class="pokeCardWrapper">
	{#each filteredPokemon as pokeman}
		<div class="pokeCard">
			<PokemanCard pokemancard={pokeman} />
		</div>
	{/each}
</div>

<style>
	.searchBarWrap {
		display: flex;
		justify-content: center;
	}
	.pokeCardWrapper {
		display: flex;
		padding: 10px;
		flex-wrap: wrap;
		justify-content: center;
	}
	.pokeCard {
		padding: 10px;
		width: fit-content;
		min-width: 350px;
	}
	.input {
		width: 76%;
		align-self: center;
	}
</style>
