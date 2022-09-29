<script lang="ts">
	import { Button } from '@svelteuidev/core';
	import { getBuckets } from './connect.svelte';

	const getAwsBuckets = () => {
		console.log('a')
	};
</script>

<Button on:click={() => getAwsBuckets()}>button</Button>
