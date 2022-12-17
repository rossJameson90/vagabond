<script>
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	let clicked = false;
	let mounted = false;
	let routeName;

	let routes = ['pokedex', 'spellbook', 'character', 'locations', 'entergame/[id]'];

	let navClick = () => {
		clicked = !clicked;
	};

	let routeTo = (e) => {
		routeName = e.path[0].id;
		goto(routeName);
		clicked = !clicked;
	};
</script>

{#if !clicked}
	<div on:click={navClick} class="mobileNavWrap {clicked ? 'clicked' : ''}">
		<div class="nav">NAV</div>
	</div>
{:else}
	<div class="unfoldedNavWrap">
		<div class="routesContainer">
			{#each routes as route}
				<div id="/{route}" on:click={routeTo} class="routeWrap">{route}</div>
			{/each}
		</div>
		<div on:click={navClick} class="close">CLOSE</div>
	</div>
{/if}

<style lang="scss">
	.mobileNavWrap {
		display: flex;
		background-color: rgba(186, 94, 219, 0.15);
		backdrop-filter: blur(5px);
		justify-content: center;
		width: 100%;
		border-radius: 8px;
		height: 40px;
	}
	.clicked {
		backdrop-filter: none;
		background-color: blue;
	}
	.unfoldedNavWrap {
		background-color: rgba(186, 94, 219, 0.15);
		backdrop-filter: blur(30px);
		padding: 10px;
		border-radius: 8px;
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		height: 100%;
		width: 100%;
		align-items: center;
		display: flex;
		justify-content: space-evenly;
		flex-wrap: wrap;
		z-index: 1000;
	}
	.routeWrap {
		height: 100px;
		width: 100px;
		background-color: red;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 8px;
		margin: 5px;
	}
	.routesContainer {
		width: 100%;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
		max-width: 329px;
	}
	.close {
		width: 100%;
		background-color: red;
		border-radius: 8px;
		padding: 10px;
		display: flex;
		justify-content: center;
		margin: 5px;
	}
</style>
