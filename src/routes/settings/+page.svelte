<script lang="ts">
	import { browser } from '$app/environment';
	import { goto } from '$app/navigation';
	import Topbar from '$lib/components/Topbar.svelte';

	function reserveScooter() {
		if (browser) {
			localStorage.clear();
			localStorage.setItem('scooterName', 'TEST');
			localStorage.setItem('reservationTimestamp', Date.now().toString());
		}
	}
</script>

<Topbar label="Einstellungen" />

<div
	class="flex w-full flex-col divide-y divide-(--color-border) rounded-2xl border border-(--color-border) bg-(--color-background-alt) text-(--color-text)"
>
	<span
		class="cursor-pointer rounded-t-xl p-4 hover:bg-(--color-border)"
		role="none"
		onclick={() => {
			reserveScooter();
			goto('/');
		}}>Reservierung simulieren</span
	>
	<div
		class="flex cursor-pointer items-center justify-between rounded-b-xl p-4 hover:bg-(--color-border)"
		role="none"
		onclick={() => {
			window.open('https://github.com/Geeked-IT-Solutions/scooteq-app');
		}}
	>
		<span>Quellcode</span>
		<span class="bi bi-box-arrow-up-right text-lg"></span>
	</div>
</div>
