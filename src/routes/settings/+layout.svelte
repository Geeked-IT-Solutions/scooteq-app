<script lang="ts">
	let { children } = $props();
</script>

<div class="flex flex-col gap-4 p-4">
	{@render children()}
</div>

{#if true}
	<style>
		body {
			background-color: var(--color-background);
		}
	</style>
{/if}
