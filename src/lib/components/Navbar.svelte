<script>
	import { url } from 'inspector';
	import Icon from './Icon.svelte';

	const navbarItems = [
		{ key: 'house', label: 'Start', url: '/' },
		{ key: 'gear', label: 'Einstellungen', url: '/settings' }
	];
</script>

<nav
	class="fixed bottom-0 left-0 z-999 flex h-16 w-full items-center justify-center border-t border-gray-200 bg-gray-100 px-4"
>
	{#each navbarItems as item}
		<a
			href={item.url}
			class="mx-2 flex w-20 cursor-pointer flex-col items-center justify-center text-2xl"
		>
			<Icon key={item.key} />
			<span class="text-xs wrap-anywhere">{item.label}</span>
		</a>
	{/each}
</nav>
