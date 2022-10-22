<script lang="ts">
	import Textfield from '@smui/textfield';
	import Button, { Label } from '@smui/button';
	import Accordion, { Panel, Header, Content } from '@smui-extra/accordion';

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

<center>
	<!-- <Stack override={{ width: 500, margin: 50 }}> -->
	<Textfield label="AWS Access key ID" bind:value={accessKey} />
	<Textfield label="Endpoint URL" bind:value={endpoint} />
	<Textfield label="AWS Secret Key" bind:value={secretKey} />

	<Button variant="raised" on:click={bucket}>Connect</Button>

	<br />
	{#if startWaiting}
		{#await bucketInside}
			<center> pls wait </center>
		{:then value}
			{#each value.Buckets as item}
				<Accordion>
					<Panel>
						<Header
							><div class="mdc-typography--headline1">{item.Name}</div>
							Created {item.CreationDate}</Header
						>
						<Content>
							Here Will be the contents of bucket {item.Name}
						</Content>
					</Panel>
				</Accordion>
			{/each}
		{/await}
	{/if}
	<!-- </Stack> -->
</center>
