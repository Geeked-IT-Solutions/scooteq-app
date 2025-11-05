<script lang="ts">
	import Navbar from '$lib/components/Navbar.svelte';
	import '../app.css';

	let { children } = $props();
</script>

<div class="pt-12 pb-18">
	{@render children()}
</div>
<Navbar />
