<script lang="ts">
	import { browser } from '$app/environment';
	import { scooters } from '$lib/data';

	let name: string | null = $state('');
	let reservationTimestamp: string | null = $state('');
	let elapsedTime: number = $state(0);
	let price: string | null = $state('');

	let reservationDialogOpen: boolean = $state(false);
	let activationCode: string = $state('');

	let endReservationDialogOpen: boolean = $state(false);
	let finalPrice: string | null = $state('');
	let finalElapsedTime: string | null = $state('');

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

	$effect(() => {
		activationCode = activationCode.trim().toUpperCase();
	});

	if (browser) loadReservationData();
</script>

{#if name}
	<div
		class="fixed inset-x-0 bottom-24 z-999 mx-2 flex items-center justify-between rounded-2xl border border-(--color-border) bg-white p-2 pl-4"
	>
		<span class="font-bold">{name}</span>
		<span>{formatTime(elapsedTime)}</span>
		<button
			class="cursor-pointer rounded-md bg-red-500 p-2 text-white"
			onclick={() => {
				endReservationDialogOpen = true;
				finalPrice = price;
				finalElapsedTime = formatTime(elapsedTime);
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
		class="fixed right-2 bottom-24 z-999 flex cursor-pointer items-center justify-between rounded-2xl bg-(--color-primary) p-4 text-(--color-background) shadow-lg"
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
		class="fixed inset-0 z-1000 flex flex-col items-center justify-center gap-1 bg-black/80 px-4 text-lg text-(--color-background)"
		onclick={() => {
			activationCode = '';
			reservationDialogOpen = false;
		}}
		role="none"
	>
		<span class="bi bi-x fixed top-4 right-4 z-1000 cursor-pointer text-3xl"></span>
		<div class="flex w-full items-center gap-4">
			<input
				type="text"
				class="w-full border-b border-(--color-background)"
				placeholder="Reservierungscode"
				bind:value={activationCode}
				onclick={(e) => e.stopPropagation()}
				onkeydown={(e) => {
					if (e.key === 'Enter') reserveScooter();
				}}
			/>
			<button
				class="bi bi-arrow-right cursor-pointer rounded-md bg-(--color-secondary) px-4 py-2"
				aria-label="Reservieren"
				onclick={(e) => {
					e.stopPropagation();
					reserveScooter();
				}}
			></button>
		</div>
		<span class="w-full text-left text-sm">1 € Entsperrgebühr + 0,23 €/min</span>
	</div>
{/if}

{#if endReservationDialogOpen}
	<div
		class="fixed inset-0 z-1000 flex flex-col items-center justify-center gap-2 bg-black/80 px-4 text-lg text-(--color-background)"
		onclick={() => {
			endReservationDialogOpen = false;
		}}
		role="none"
	>
		<span class="bi bi-x fixed top-4 right-4 z-1000 cursor-pointer text-3xl"></span>
		<span class="font-bold">Fahrt abgeschlossen</span>
		<span>Fahrtdauer: <span class="font-bold">{finalElapsedTime}</span></span>
		<span>Fahrtkosten: <span class="font-bold">{finalPrice}</span></span>
	</div>
{/if}
