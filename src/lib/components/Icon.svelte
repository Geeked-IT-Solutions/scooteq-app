<script lang="ts">
	interface IconProps {
		key: string;
		onClick?: () => void;
	}

	let { key, onClick }: IconProps = $props();
</script>

<span class="bi bi-{key}" onclick={onClick} role="none"></span>
