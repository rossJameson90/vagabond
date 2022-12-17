<script language="ts">
	import Map from '$lib/Map/Map.svelte';
	// import { mapInfo } from '../../stores/index';
	// import { size, slice } from 'lodash-es';

	let mode = 'map';
	let filteredLocations;

	// $: filteredLocations = slice($mapInfo, 0, 60);
</script>

<div class="h-60 mapContainer">
	<Map mapData={filteredLocations} />
</div>

<style language="scss">
	.mapContainer {
		margin-top: 20px;
	}
</style>
