<script lang="ts">
	import { onMount, onDestroy, getContext, setContext } from 'svelte';
	import L from 'leaflet';
	import 'leaflet/dist/leaflet.css';

    export let customImg: boolean;
	export let latLng: L.LatLngExpression;

	let marker: L.Marker | undefined;
	let markerElement: HTMLElement;

	const { getMap }: { getMap: () => L.Map | undefined } = getContext('map');
	const map = getMap();

	setContext('layer', {
		// L.Marker inherits from L.Layer
		getLayer: () => marker
	});

	onMount(() => {
		if (map) {
            if (customImg){
                let icon = L.divIcon({
                    html: markerElement,
                    className: 'map-marker',
                    iconAnchor: [20, 25]
                });
                marker = L.marker(latLng, { icon }).addTo(map);

            } else {
			    marker = L.marker(latLng).addTo(map);
            }
		}
	});

	onDestroy(() => {
		marker?.remove();
		marker = undefined;
	});
</script>

<div bind:this={markerElement}>
	{#if marker}
		<slot />
	{/if}
</div>