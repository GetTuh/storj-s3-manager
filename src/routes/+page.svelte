<script lang="ts">
	import Textfield from '@smui/textfield';
	import Button, { Label } from '@smui/button';
	import Accordion, { Panel, Header, Content } from '@smui-extra/accordion';
	import LinearProgress from '@smui/linear-progress';

	import { onMount } from 'svelte';
	import { getBuckets, getItemsFromBucket } from './connect.svelte';
	let accessKey: string, endpoint: string, secretKey: string;
	let waitForBuckets = false;
	let waitForItem = false;
	let bucketItems: any;
	let bucketInside: any;
	const bucket = async () => {
		waitForBuckets = true;
		let bucket = getBuckets(endpoint, accessKey, secretKey);
		bucketInside = bucket;
	};
	let bucketFiles = async (bucketName: string) => {
		waitForItem = true;
		let items = getItemsFromBucket(bucketName, endpoint, accessKey, secretKey);
		bucketItems = await items;
		console.log(bucketItems);
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

<center>
	<Textfield label="AWS Access key ID" bind:value={accessKey} />
	<Textfield label="Endpoint URL" bind:value={endpoint} />
	<Textfield label="AWS Secret Key" bind:value={secretKey} />

	<Button variant="raised" on:click={bucket}>Connect</Button>

	<br />
	{#if waitForBuckets}
		{#await bucketInside}
			<div class="mdc-typography--headline1">Getting data...</div>
			<br />
			<LinearProgress indeterminate />
		{:then value}
			{#each value.Buckets as bucket}
				<Accordion>
					<Panel on:click={() => bucketFiles(bucket.Name)}>
						<Header
							><div class="mdc-typography--headline1">{bucket.Name}</div>
							Created {bucket.CreationDate}</Header
						>
						<Content>
							{#if waitForItem}
								{#await bucketItems}
									<div class="mdc-typography--headline1">Getting Bucket Items</div>
									<br />
									<LinearProgress indeterminate />
								{:then items}
									{#each items.Contents as item}
										{item}
									{/each}
								{/await}
								elo
							{/if}
						</Content>
					</Panel>
				</Accordion>
			{/each}
		{/await}
	{/if}
	<!-- </Stack> -->
</center>
