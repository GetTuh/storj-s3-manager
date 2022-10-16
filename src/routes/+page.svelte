<script lang="ts">
	import {
		Button,
		Input,
		Center,
		Stack,
		Group,
		Space,
		Text,
		Title,
		exception,
		Loader
	} from '@svelteuidev/core';
	import { onMount } from 'svelte';

	import { getBuckets, getItemsFromBucket } from './connect.svelte';
	let accessKey: string, endpoint: string, secretKey: string;
	let startWaiting = false;
	let bucketInside: any;
	let bucket = async () => {
		startWaiting = true;
		let bucket = getBuckets(endpoint, accessKey, secretKey);
		bucketInside = bucket;
	};
	// const setLocalStorageVars = () => {
	// 	localStorage.setItem('endpoint', endpoint);
	// 	localStorage.setItem('accessKey', accessKey);
	// 	localStorage.setItem('secretKey', secretKey);
	// };
	try {
		accessKey = localStorage.getItem('accessKey');
		endpoint = localStorage.getItem('endpoint');
		secretKey = localStorage.getItem('secretKey');
	} catch {
		console.log('No data saved');
	}
</script>

<Center>
	<Stack override={{ width: 500, margin: 50 }}>
		<Input placeholder="AWS Access key ID" bind:value={accessKey} />
		<Input placeholder="Endpoint URL" bind:value={endpoint} />
		<Input placeholder="AWS Secret Key" bind:value={secretKey} />

		<Button on:click={bucket}>Connect</Button>

		<!-- <Button on:click={()=>data=1}>show data</Button> -->
		{#if startWaiting}
			{#await bucketInside}
				<Center>
					<Loader />
				</Center>
			{:then value}
				{#each value.Buckets as item}
					<Group grow>
						<Button
							variant="outline"
							on:click={() => getItemsFromBucket(item.Name)}
							override={{ height: 50 }}
							><Title>{item.Name}</Title>
							<Space />
							<Text align="right">Created {item.CreationDate}</Text>
						</Button>
					</Group>
				{/each}
			{/await}
		{/if}
	</Stack>
</Center>
