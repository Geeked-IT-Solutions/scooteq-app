<script lang="ts">
	import Icon from './Icon.svelte';

	interface TopbarProps {
		icon?: string;
		label?: string;
	}

	let { icon, label }: TopbarProps = $props();

	let menuOpen = $state(false);
</script>

<div
	class="fixed top-0 left-0 z-999 flex h-12 w-full items-center justify-center bg-[#1b3855] px-4"
>
	<div class="flex items-center gap-2 text-xl text-yellow-500">
		<Icon key={icon ?? 'scooter'} />
		<span class="text-lg font-semibold text-white">{label ?? 'ScooTeq'}</span>
	</div>
</div>
