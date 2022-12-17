<script>
	import mapboxgl from 'mapbox-gl';
	import { onMount } from 'svelte';
	import customWorldMap from './fullMap.jpg';

	const DEFAULT_ZOOM = 5;
	const DEFAULT_LNG = 37.015;
	const DEFAULT_LAT = 39.7505;
	const CONTAINER_ID = 'directory-map';

	export let mapData = [];

	const geojson = {
		type: 'FeatureCollection',
		features: [
			{
				type: 'Feature',
				geometry: {
					type: 'Point',
					coordinates: [37.015, 39.7505]
				},
				properties: {
					title: 'Mapbox',
					description: 'Kisbee.'
				}
			},
			{
				type: 'Feature',
				geometry: {
					type: 'Point',
					coordinates: [36.336, 41.2797]
				},
				properties: {
					title: 'Mapbox',
					description: 'Tyr'
				}
			},
			{
				type: 'Feature',
				geometry: {
					type: 'Point',
					coordinates: [36.1756, 36.5847]
				},
				properties: {
					title: 'Mapbox',
					description: 'Ank.'
				}
			},
			{
				type: 'Feature',
				geometry: {
					type: 'Point',
					coordinates: [35.482, 38.7205]
				},
				properties: {
					title: 'Mapbox',
					description: 'Bedlum.'
				}
			},
			{
				type: 'Feature',
				geometry: {
					type: 'Point',
					coordinates: [29.2842, 40.6549]
				},
				properties: {
					title: 'Mapbox',
					description: 'An-kara.'
				}
			}
		]
	};

	let map;
	let innerWidth;
	let show = false;

	let click = () => {
		show = !show;
	};

	onMount(async () => {
		mapboxgl.accessToken =
			'pk.eyJ1Ijoicm9zY28yMzQyIiwiYSI6ImNsMm00ZWp3ZDB0eHMza214emhsZnpoem8ifQ.HWX6QvBKVAqQcPa8MCM7gQ';
		map = new mapboxgl.Map({
			container: CONTAINER_ID,
			style: 'mapbox://styles/mapbox/satellite-v9',
			center: [37.015, 39.7505],
			zoom: 4.5
		});

		map.on('load', async () => {
			console.log('map data == ', mapData);

			map.addSource('points', {
				type: 'geojson',
				data: geojson
			});

			map.addLayer({
				id: 'circle',
				type: 'circle',
				source: 'points',
				paint: {
					'circle-color': '#4264fb',
					'circle-radius': 8,
					'circle-stroke-width': 2,
					'circle-stroke-color': '#ffffff'
				}
			});

			// Center the map on the coordinates of any clicked circle from the 'circle' layer.
			map.on('click', 'circle', (e) => {
				map.flyTo({
					center: e.features[0].geometry.coordinates
				});
				console.log('e == ', e);
				// If the user clicked on one of your markers, get its information.
				const features = map.queryRenderedFeatures(e.point, {
					layers: ['circle'] // replace with your layer name
				});
				if (!features.length) {
					return;
				}
				const feature = features[0];
				console.log('feature == ', feature);
				const popup = new mapboxgl.Popup({ offset: [0, -15] })
					.setLngLat(feature.geometry.coordinates)
					.setHTML(`<p>${feature.properties.description}</p>`)
					.addTo(map);
			});

			// Change the cursor to a pointer when the it enters a feature in the 'circle' layer.
			map.on('mouseenter', 'circle', () => {
				map.getCanvas().style.cursor = 'pointer';
			});

			// Change it back to a pointer when it leaves.
			map.on('mouseleave', 'circle', () => {
				map.getCanvas().style.cursor = '';
			});
		});
	});
</script>

<svelte:window bind:innerWidth />
<svelte:head>
	<link href="https://api.mapbox.com/mapbox-gl-js/v2.8.1/mapbox-gl.css" rel="stylesheet" />
</svelte:head>
<div class="flex flex-col justify-center">
	<div id={CONTAINER_ID} class="self-center mapContainer" />
	<span class="bottomMapButton">
		<button on:click={click} class="bg-blue-500 p-4 rounded h-18 w-20 ">Show Ancient Map</button>
	</span>
	{#if show}
		<div class="bottomMap">
			<img src={customWorldMap} alt="" />
		</div>
	{/if}
</div>

<style language="scss">
	.bottomMapButton {
		display: flex;
		justify-content: center;
		margin: 10px;
	}
	button {
		width: 100%;
	}
	.mapContainer {
		height: 600px;
		width: 80%;
		border-radius: 8px;
	}
</style>
