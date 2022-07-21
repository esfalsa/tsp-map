<script>
	import Map from '../components/map.svelte';
	import { data } from '$lib/data.js';

	let selectedPlot = null;
	$: selected = data[selectedPlot];
</script>

<h1 class="text-3xl font-bold">Map of the South Pacific</h1>

<div class="flex flex-row">
	<div class="flex-1">
		<Map bind:selectedPlot />
	</div>
	<div class="flex-1">
		<div id="infoDisplay">
			{#if selected}
				<h3 class="text-3xl font-extrabold" id="name">{selected.name}</h3>
				<p><strong>Plot Number:</strong> {selectedPlot}</p>
			{:else if selectedPlot}
				<p><strong>Plot Number:</strong> {selectedPlot}</p>
			{/if}
		</div>
	</div>
</div>
