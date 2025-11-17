<script lang="ts">
	import { browser } from '$app/environment';
	let name: string | null = $state('');
	let reservationTimestamp: string | null = $state('');
	let elapsedTime: number = $state(0);
	let price: string | null = $state('');
	let paymentType: string | null = $state('');

	if (browser) {
		name = localStorage.getItem('scooterName');
		reservationTimestamp = localStorage.getItem('reservationTimestamp');
		paymentType = localStorage.getItem('paymentType');
	}

	setInterval(() => {
		elapsedTime = Math.floor((Date.now() - Number(reservationTimestamp)) / 1000);
		if (browser) {
			if (paymentType === 'per_minute') {
				// only update price every minute
				price = (0.23 * Math.ceil(elapsedTime / 60)).toLocaleString('de-DE', {
					style: 'currency',
					currency: 'EUR'
				});
			} else {
				// preis ist 39 cent pro kilometer, durchschnittliche geschwindigkeit 15 km/h => 12.87 cent pro minute
				price = (0.1287 * Math.ceil(elapsedTime / 60)).toLocaleString('de-DE', {
					style: 'currency',
					currency: 'EUR'
				});
			}
		}
	}, 1000);

	function formatTime(seconds: number) {
		const minutes = Math.floor(seconds / 60);
		const secs = seconds % 60;
		return `${String(minutes).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
	}
</script>

{#if name}
	<div
		class="fixed inset-x-0 bottom-22 z-999 mx-2 flex items-center justify-between rounded-2xl border border-(--color-border) bg-white p-4"
	>
		<span>{formatTime(elapsedTime)}</span>
		<span>{price}</span>
		<button>Beenden</button>
	</div>
{:else}
	<div
		class="fixed right-2 bottom-22 z-999 flex items-center justify-between rounded-2xl bg-(--color-primary) p-4 text-(--color-background) shadow-lg"
	>
		Reservieren
	</div>
{/if}
