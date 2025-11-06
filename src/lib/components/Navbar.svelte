<script lang="ts">
	import { goto } from '$app/navigation';
	import { page } from '$app/state';
	import Icon from './Icon.svelte';

	const navbarItems = [
		{ key: 'map', label: 'Karte', url: '/' },
		{ key: 'calculator', label: 'Preisrechner', url: '/calculator' },
		{ key: 'gear', label: 'Einstellungen', url: '/settings' }
	];

	const activeSegment = $derived('/' + page.url.pathname.split('/')[1]);
	const handleNavigate = (url: string | URL) => goto(url);
</script>

<nav
	class="fixed inset-x-0 bottom-0 z-999 flex h-20 items-center justify-center border-t border-(--color-border) bg-(--color-box)/80 px-4 backdrop-blur-lg"
>
	{#each navbarItems as item}
		<div
			class="mx-2 flex h-full w-20 cursor-pointer flex-col items-center justify-center text-2xl"
			class:text-(--color-primary)={item.url === activeSegment}
			class:font-semibold={item.url === activeSegment}
			role="none"
			onclick={() => {
				handleNavigate(item.url);
			}}
		>
			<Icon key={item.key} />
			<span class="text-xs wrap-anywhere">{item.label}</span>
		</div>
	{/each}
</nav>
