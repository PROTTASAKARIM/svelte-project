<!-- Map.svelte -->
<script>
	import { onMount } from 'svelte';
	import Map from 'ol/Map';
	import View from 'ol/View';
	import TileLayer from 'ol/layer/Tile';
	import OSM from 'ol/source/OSM';
	import VectorLayer from 'ol/layer/Vector';
	import VectorSource from 'ol/source/Vector';
	import GeoJSON from 'ol/format/GeoJSON';
	import Style from 'ol/style/Style';
	import Fill from 'ol/style/Fill';
	import Stroke from 'ol/style/Stroke';

	let map;

	onMount(() => {
		// Define the custom style for the GeoJSON layer
		const geojsonStyle = new Style({
			fill: new Fill({
				color: 'rgba(0, 106, 78, 0.75)' // Color in RGBA format
			}),
			stroke: new Stroke({
				color: '#006a4e', // Color set to #006a4e
				width: 2,
				opacity: 0.75 // Opacity set to 0.75
			})
		});

		// Create a new VectorSource with the custom style
		const vectorSource = new VectorSource({
			format: new GeoJSON(),
			url: '../../lib/countries.geojson'
		});

		vectorSource.on('change', () => {
			const state = vectorSource.getState();
			if (state === 'ready') {
				console.log('GeoJSON data loaded:', vectorSource.getFeatures());
				// Now you can work with the loaded data.
			} else if (state === 'loading') {
				console.log('Loading data...');
			} else if (state === 'error') {
				console.error('Error loading GeoJSON data');
			}
		});

		console.log('vectorSource', vectorSource);
		// Create a new VectorLayer and apply the custom style
		const vectorLayer = new VectorLayer({
			source: vectorSource,
			style: geojsonStyle
		});

		// Create a new map instance
		map = new Map({
			target: 'map',
			layers: [
				new TileLayer({
					source: new OSM()
				}),
				vectorLayer // Add the GeoJSON layer to the map
			],
			view: new View({
				center: [0, 0],
				zoom: 2
			})
		});
	});
</script>

<div id="map" />

<style>
	/* Define CSS styles for the map container */
	#map {
		width: 100%;
		height: 100vh; /* Use 100vh to make it full-screen height */
	}
</style>
