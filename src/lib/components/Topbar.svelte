<script>
	import Icon from './Icon.svelte';

	let menuOpen = $state(false);
</script>

<div
	class="fixed top-0 left-0 z-999 flex h-12 w-full items-center justify-center bg-[#1b3855] px-4"
>
	<div class="text-xl text-yellow-500">
		<Icon key="scooter" />
		<span class="ml-1 text-lg font-semibold text-white">ScooTeq</span>
	</div>
</div>
