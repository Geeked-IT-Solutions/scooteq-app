<script lang="ts">
	import ScooterDashboard from '$lib/components/ScooterDashboard.svelte';
	import Topbar from '$lib/components/Topbar.svelte';
	import { onMount, onDestroy } from 'svelte';
	let mapEl: HTMLDivElement;

	let L: typeof import('leaflet') | null = null;

	let follow = true;
	let minZoom = 14;
	let maxZoom = 18;
	let initialZoom = 16;

	let map: any;
	let accuracyCircle: any;
	let watchId: number | null = null;

	function setPosition(lat: number, lng: number, accuracy?: number) {
		const latlng = [lat, lng] as [number, number];

		if (accuracy !== undefined) {
			if (!accuracyCircle) {
				accuracyCircle = L!.circle(latlng, { radius: accuracy, fillOpacity: 0.8 });
				accuracyCircle.addTo(map);
			} else {
				accuracyCircle.setLatLng(latlng);
				accuracyCircle.setRadius(accuracy);
			}
		}

		if (follow && !map.getBounds().contains(latlng)) {
			map.panTo(latlng, { animate: true });
		}
	}

	onMount(async () => {
		if (typeof window === 'undefined') return;
		L = await import('leaflet');

		map = L!.map(mapEl, { zoomControl: false }).setView([0, 0], initialZoom);

		L!
			.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
				minZoom,
				maxZoom,
				attribution: '&copy; OpenStreetMap contributors'
			})
			.addTo(map);

		navigator.geolocation.getCurrentPosition(
			(pos) => {
				const { latitude, longitude, accuracy } = pos.coords;
				map.setView([latitude, longitude], initialZoom);
				setPosition(latitude, longitude, accuracy);
			},
			(err) => {
				console.warn('getCurrentPosition error:', err);
			},
			{ enableHighAccuracy: true, timeout: 10000, maximumAge: 0 }
		);

		watchId = navigator.geolocation.watchPosition(
			(pos) => {
				const { latitude, longitude, accuracy } = pos.coords;
				setPosition(latitude, longitude, accuracy);
			},
			(err) => {
				console.warn('watchPosition error:', err);
			},
			{ enableHighAccuracy: true, timeout: 15000, maximumAge: 0 }
		);
	});

	onDestroy(() => {
		if (watchId !== null) navigator.geolocation.clearWatch(watchId);
		if (map) map.remove();
	});
</script>

<Topbar />

<div
	bind:this={mapEl}
	id="map"
	class="fixed inset-0 z-0
"
></div>
<ScooterDashboard />

<!--
<iframe
	class="h-screen w-full"
	title="Karte"
	src="https://www.openstreetmap.org/export/embed.html?bbox=-7.27294921875%2C44.63739123445587%2C28.1689453125%2C57.16007826737998&amp;layer=mapnik"
></iframe>
-->
