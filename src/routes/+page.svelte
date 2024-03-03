<!-- YOU CAN DELETE EVERYTHING IN THIS PAGE -->
<!--
<div class="container h-full mx-auto flex justify-center items-center">
	<div class="space-y-5">
		<h1 class="h1">Let's get cracking bones!</h1>
		<p>Start by exploring:</p>
		<ul>
			<li><code class="code">/src/routes/+layout.svelte</code> - barebones layout</li>
			<li><code class="code">/src/app.postcss</code> - app wide css</li>
			<li>
				<code class="code">/src/routes/+page.svelte</code> - this page, you can replace the contents
			</li>
		</ul>
	</div>
</div>
-->

<script lang="ts">
	
	import type { LatLngExpression } from 'leaflet';
	import Leaflet from '$lib/Leaflet.svelte';
	const initialView: LatLngExpression = [51.514244, 7.468429]; // Dortmund, Germany

</script>

<div class="w-full h-screen">
	<Leaflet view={initialView} zoom={14}>
	</Leaflet>
</div>