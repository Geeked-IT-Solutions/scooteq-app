<script lang="ts">
	import { browser } from '$app/environment';
	import { scooters } from '$lib/data';

	let name: string | null = $state('');
	let reservationTimestamp: string | null = $state('');
	let elapsedTime: number = $state(0);
	let price: string | null = $state('');

	let reservationDialogOpen: boolean = $state(false);
	let activationCode: string = $state('');

	const scooterActivationCodes = scooters.map((scooter) => scooter.activationCode);

	function loadReservationData() {
		name = localStorage.getItem('scooterName');
		reservationTimestamp = localStorage.getItem('reservationTimestamp');
	}

	function reserveScooter() {
		if (!scooterActivationCodes.includes(activationCode.trim())) {
			alert('Ungültiger Reservierungscode');
			return;
		}

		const scooterName = scooters.find(
			(scooter) => scooter.activationCode === activationCode.trim()
		)?.name;

		localStorage.setItem('scooterName', scooterName!);
		localStorage.setItem('reservationTimestamp', Date.now().toString());

		reservationDialogOpen = false;
		activationCode = '';

		loadReservationData();
	}

	function formatTime(seconds: number) {
		const minutes = Math.floor(seconds / 60);
		const secs = seconds % 60;
		return `${String(minutes).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
	}

	setInterval(() => {
		elapsedTime = Math.floor((Date.now() - Number(reservationTimestamp)) / 1000);
		if (browser) {
			price = (1 + 0.23 * Math.ceil(elapsedTime / 60)).toLocaleString('de-DE', {
				style: 'currency',
				currency: 'EUR'
			});
		}
	}, 1000);

	if (browser) loadReservationData();
</script>

{#if name}
	<div
		class="fixed inset-x-0 bottom-22 z-999 mx-2 flex items-center justify-between rounded-2xl border border-(--color-border) bg-white p-2 pl-4"
	>
		<span class="font-bold">{name}</span>
		<span>{formatTime(elapsedTime)}</span>
		<button
			class="cursor-pointer rounded-md bg-red-500 p-2 text-white"
			onclick={() => {
				localStorage.clear();
				name = '';
				reservationTimestamp = '';
				elapsedTime = 0;
				price = '';
			}}>Beenden</button
		>
	</div>
{:else}
	<div
		class="fixed right-2 bottom-22 z-999 flex cursor-pointer items-center justify-between rounded-2xl bg-(--color-primary) p-4 text-(--color-background) shadow-lg"
		role="none"
		onclick={() => {
			reservationDialogOpen = true;
		}}
	>
		Reservieren
	</div>
{/if}

{#if reservationDialogOpen}
	<div
		class="fixed inset-0 z-1000 flex items-center justify-center gap-2 bg-black/80 px-4 text-lg"
		onclick={() => {
			activationCode = '';
			reservationDialogOpen = false;
		}}
		role="none"
	>
		<input
			type="text"
			class="w-full border-b border-(--color-background) text-(--color-background)"
			placeholder="Reservierungscode"
			bind:value={activationCode}
			onclick={(e) => e.stopPropagation()}
			onkeydown={(e) => {
				if (e.key === 'Enter') reserveScooter();
			}}
		/>
		<button
			class="bi bi-arrow-right cursor-pointer rounded-md bg-(--color-secondary) p-2 text-(--color-background)"
			aria-label="Reservieren"
			onclick={(e) => {
				e.stopPropagation();
				reserveScooter();
			}}
		></button>
	</div>
{/if}
