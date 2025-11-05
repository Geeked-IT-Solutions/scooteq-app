<script>
	import { goto } from '$app/navigation';
	import Icon from './Icon.svelte';

	const navbarItems = [
		{ key: 'map', label: 'Karte', url: '/' },
		{ key: 'calculator', label: 'Preisrechner', url: '/calculator' },
		{ key: 'gear', label: 'Einstellungen', url: '/settings' }
	];
</script>

<nav
	class="fixed inset-x-0 bottom-0 z-999 flex h-18 items-center justify-center border-t border-(--color-border) bg-(--color-box)/80 px-4"
>
	{#each navbarItems as item}
		<div
			class="mx-2 flex w-20 cursor-pointer flex-col items-center justify-center text-2xl"
			role="none"
			onclick={() => {
				goto(item.url);
			}}
		>
			<Icon key={item.key} />
			<span class="text-xs wrap-anywhere">{item.label}</span>
		</div>
	{/each}
</nav>
