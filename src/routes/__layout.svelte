<script>
	import { supabase } from '../lib/supabaseClient.js';
	import Header from '$lib/header/Header.svelte';
	import MobileNav from '../components/MobileNav/MobileNav.svelte';
	import '../app.css';

	supabase;
	console.log(supabase);

	$: outerWidth = 0;
	$: innerWidth = 0;
	$: outerHeight = 0;
	$: innerHeight = 0;
	$: console.log('inner width == ', innerWidth);
	$: console.log('outer width == ', outerWidth);
</script>

<svelte:window bind:innerWidth bind:outerWidth bind:innerHeight bind:outerHeight />

{#if innerWidth >= 778}
	<Header />
{:else}
	<MobileNav />
{/if}

<main>
	<!-- this slot allows the lay out to affect all route pages  -->
	<slot />
</main>

<footer>
	<p>Developed by Rosco</p>
</footer>

<style>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
	/* main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 1024px;
		margin: 0 auto;
		box-sizing: border-box;
	} */

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 40px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 40px 0;
		}
	}
</style>
