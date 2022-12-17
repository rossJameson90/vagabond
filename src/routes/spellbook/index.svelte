<script>
	import { spells } from '../../stores/spellstore';
	import SpellCard from '../../components/spellCard.svelte';

	let searchTerm = '';
	let filteredSpells = [];

	$: {
		console.log(searchTerm);
		if (searchTerm) {
			filteredSpells = $spells.filter((spell) =>
				spell.name.toLowerCase().includes(searchTerm.toLowerCase())
			);
		} else {
			filteredSpells = [...$spells];
		}
	}
</script>

<h1 class="text-4xl text-center my-8 uppercase">svelte pokedex</h1>
<div class="inputWrap">
	<input
		class="input rounded-md text-lg p-4 border-2 border-gray-200"
		type="text"
		placeholder="search spells"
		bind:value={searchTerm}
	/>
</div>

<!-- <div class="py-4 grid gap-4 md:grid-cols-2 grid-cols-1"> -->
<div class="cardWrapper">
	{#each filteredSpells as spell}
		<div class="cards">
			<SpellCard spellcard={spell} />
		</div>
	{/each}
</div>

<!-- </div> -->
<style lang="scss">
	.cardWrapper {
		justify-content: center;
		display: flex;
		padding: 10px;
		flex-wrap: wrap;
	}
	.cards {
		padding: 10px;
		width: fit-content;
		min-width: 300px;
	}
	.cards:hover {
		background-color: darkseagreen;
		border-radius: 12px;
		text-decoration: none;
	}
	.inputWrap {
		display: flex;
		justify-content: center;
	}
	.input{
		width: 67%;
	}
</style>
