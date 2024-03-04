<script lang="ts">
	import { onMount, onDestroy, getContext, setContext } from 'svelte';
	import L from 'leaflet';
	import 'leaflet/dist/leaflet.css';
    
    //import iconRetinaUrl from '$lib/assets/marker-icon-2x.png';
    //import iconUrl from '$lib/assets/marker-icon.png';
    //import shadowUrl from '$lib/assets/marker-shadow.png';

    import iconRetinaUrl from '/images/marker-icon-2x.png';
    import iconUrl from '/images/marker-icon.png';
    import shadowUrl from '/images/marker-shadow.png';


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
                const iconDefault = L.icon({
                    iconRetinaUrl,
                    iconUrl,
                    shadowUrl,
                    iconSize: [25, 41],
                    iconAnchor: [12, 41],
                    popupAnchor: [1, -34],
                    tooltipAnchor: [16, -28],
                    shadowSize: [41, 41]
                });
                for (const [city, locations] of Object.entries(latLngDict)){
                    markers[city] = L.layerGroup();
                    locations.forEach(location => {
                        L.marker(location, { icon: iconDefault }).addTo(markers[city]).addTo(markers['All Participants'])
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

