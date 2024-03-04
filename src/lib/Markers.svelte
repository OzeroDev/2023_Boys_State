<script lang="ts">
	import { onMount, onDestroy, getContext, setContext } from 'svelte';
	import L from 'leaflet';


    export let customImg: boolean;
	export let latLngDict: { [city: string]: Array<L.LatLngExpression> };

    var markers: { [city: string]:L.LayerGroup} = {}
	let markerElement: HTMLElement;

	const { getMap }: { getMap: () => L.Map | undefined } = getContext('map');
	const map = getMap();

	setContext('layer', {
		// L.Marker inherits from L.Layer
		getLayer: () => markers
	});

	onMount(() => {
		if (map) {
            markers['All Participants'] = L.layerGroup().addTo(map)
            if (customImg){
                let icon = L.divIcon({
                    html: markerElement,
                    className: 'map-marker',
                    iconAnchor: [20, 25]
                });
                for (const [city, locations] of Object.entries(latLngDict)){
                    markers[city] = L.layerGroup();
                    locations.forEach(location => {
                        L.marker(location, { icon }).addTo(markers[city]).addTo(markers['All Participants'])
                    });

                }

            } else {
                for (const [city, locations] of Object.entries(latLngDict)){
                    markers[city] = L.layerGroup();
                    locations.forEach(location => {
                        L.marker(location).addTo(markers[city]).addTo(markers['All Participants'])
                    });

                }
            }
            L.control.layers(markers, {}, { collapsed:false }).addTo(map);
		}
	});

	onDestroy(() => {
        for (const [city, layerGroup] of Object.entries(markers)){
            layerGroup.remove()

        }
		//markers = undefined;
	});
</script>

<div bind:this={markerElement}>
	{#if markers}
		<slot />
	{/if}
</div>