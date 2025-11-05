<script lang="ts">
	import Topbar from '$lib/components/Topbar.svelte';
	import Navbar from '$lib/components/Navbar.svelte';
	import '../app.css';

	let { children } = $props();
</script>

<Topbar />
<div class="pt-12 pb-16">
	{@render children()}
</div>
<Navbar />
