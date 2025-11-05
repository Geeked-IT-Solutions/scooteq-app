<script lang="ts">
	import Icon from './Icon.svelte';

	interface TopbarProps {
		label?: string;
	}

	let { label }: TopbarProps = $props();

	let menuOpen = $state(false);
</script>

<div
	class="fixed inset-x-0 top-0 z-999 flex h-12 items-center justify-center bg-(--color-primary) px-4"
>
	<div class="flex items-center gap-2 text-xl text-(--color-secondary)">
		{#if !label}
			<Icon key="scooter" />
		{/if}
		<span class="text-lg font-semibold text-(--color-background)">{label ?? 'ScooTeq'}</span>
	</div>
</div>
