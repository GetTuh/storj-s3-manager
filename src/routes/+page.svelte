<script lang="ts">
	import Textfield from '@smui/textfield';
	import Button, { Label } from '@smui/button';
	import Accordion, { Panel, Header, Content } from '@smui-extra/accordion';
	import LinearProgress from '@smui/linear-progress';
	import Dialog, { Title, Actions } from '@smui/dialog';
	import List, { Item, Text } from '@smui/list';

	import { onMount } from 'svelte';
	import { getBuckets, getItemsFromBucket } from './connect.svelte';
	let accessKey: string, endpoint: string, secretKey: string;
	let waitForItem = false;
	let bucketItems: any;
	let bucketInside: any;

	let open = false;
	let clicked = 'Nothing yet.';
	const bucket = async () => {
		let bucket = getBuckets(endpoint, accessKey, secretKey);
		open = true;
		bucketInside = bucket;
	};
	let bucketFiles = async (bucketName: string) => {
		open = false;
		waitForItem = true;
		let items = getItemsFromBucket(bucketName, endpoint, accessKey, secretKey);
		bucketItems = items;
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
	<Dialog bind:open aria-labelledby="simple-title" aria-describedby="simple-content">
		{#if open}
			{#await bucketInside}
				<Title id="list-title">
					<div class="mdc-typography--headline1">Getting data...</div>

					<LinearProgress indeterminate />
				</Title>
				<br /><br />
			{:then value}
				<Title id="list-title">Choose a bucket</Title>
				<Content id="list-content">
					<List>
						{#each value.Buckets as bucket}
							<Item on:SMUI:action={() => bucketFiles(bucket.Name)}>
								<Text>
									{bucket.Name}
								</Text>
								<br />
								<!-- <Text>Created {bucket.CreationDate}</Text> -->
							</Item>
						{/each}
					</List>
				</Content>
			{/await}
		{/if}
	</Dialog>

	{#if waitForItem}
		{#await bucketItems}
			<Title>
				<div class="mdc-typography--headline1">Getting data...</div>

				<LinearProgress indeterminate />
			</Title>
		{:then items}
			<List>
				{#if items.Contents[0]}
					{#each items.Contents as item}
						<Item>
							{item.Key}
						</Item>
					{/each}
				{:else}
					<b>This bucket is empty.</b>
				{/if}

				<br />
			</List>
		{/await}
	{/if}
</center>
