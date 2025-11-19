<script lang="ts">
	import Topbar from '$lib/components/Topbar.svelte';

	let distance: number | null = $state(null);
	let price: string = $state('');

	$effect(() => {
		if (distance === null) return;
		if (distance < 0) {
			distance = 0;
		} else {
			price = (1 + 4 * distance * 0.23).toLocaleString('de-DE', {
				style: 'currency',
				currency: 'EUR'
			});
		}
	});
</script>

<Topbar label="Preisrechner" />

<div class="fixed inset-0 z-0 flex flex-col items-center justify-center gap-4 p-4">
	<input
		type="number"
		bind:value={distance}
		placeholder="Strecke in km"
		class="w-full rounded-md border border-(--color-border) p-2"
	/>

	<span class="text-sm text-(--color-text)"
		>1 € Entsperrgebühr + 0,23 €/min bei durchschnittlich 15 km/h</span
	>

	{#if distance && distance > 0}
		<span class="font-bold text-(--color-text)">{price}</span>
	{:else}
		<span class="w-full text-center text-(--color-text)">Geben Sie die Strecke in km an.</span>
	{/if}
</div>
