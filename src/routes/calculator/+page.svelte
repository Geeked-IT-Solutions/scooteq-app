<script lang="ts">
	import Topbar from '$lib/components/Topbar.svelte';

	let time: number = $state(5);
	let price: string = $state('');

	$effect(() => {
		if (time < 0) {
			time = 0;
		} else {
			price = (1 + time * 0.23).toLocaleString('de-DE', {
				style: 'currency',
				currency: 'EUR'
			});
		}
	});
</script>

<Topbar label="Preisrechner" />

<!--Zeiteingabe in minuten und preis berechnen knopf-->
<div class="fixed inset-0 z-0 flex flex-col items-center justify-center gap-4 p-4">
	<input
		type="number"
		bind:value={time}
		placeholder="Zeit in Minuten"
		class="w-full rounded-md border border-(--color-border) p-2"
	/>

	<span class="text-sm text-(--color-text)">1€ Entsperrgebühr + 0,23 €/min</span>

	{#if time > 0}
		<span class="font-bold text-(--color-text)">{price}</span>
	{:else}
		<span class="w-full text-center text-(--color-text)"
			>Geben Sie die geschätzte Fahrtdauer an.</span
		>
	{/if}
</div>
