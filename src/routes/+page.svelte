<script lang="ts">
	import { Button } from '@svelteuidev/core';
	import { Input, InputWrapper } from '@svelteuidev/core';
	import { Center } from '@svelteuidev/core';
	import { Grid } from '@svelteuidev/core';

	import { getBuckets } from './connect.svelte';
	let accessKey: string, endpoint: string, secretKey: string;
	const getAwsBuckets = async () => {
		const buckets = await getBuckets(endpoint, accessKey, secretKey);
		console.log(buckets);
	};
</script>

<Center>
	<Grid>
		<Grid.Col><Input placeholder="AWS Access key ID" bind:value={accessKey} /></Grid.Col>
		<Grid.Col><Input placeholder="Endpoint URL" bind:value={endpoint} /></Grid.Col>
		<Grid.Col><Input placeholder="AWS Secret Key" bind:value={secretKey} /></Grid.Col>

		<Grid.Col>
			<Button on:click={() => getAwsBuckets()}>Connect</Button></Grid.Col
		>
	</Grid>
</Center>
