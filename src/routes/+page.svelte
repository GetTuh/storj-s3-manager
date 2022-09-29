<script lang="ts">
	import { Button, Input, Center, Stack, Group, Space, Text, Title } from '@svelteuidev/core';

	import { getBuckets } from './connect.svelte';
	let accessKey: string, endpoint: string, secretKey: string;
	// const setLocalStorageVars=()=>{
	// 	localStorage.setItem("endpoint",endpoint)
	// 	localStorage.setItem("accessKey",accessKey)
	// 	localStorage.setItem("secretKey",secretKey)
	// }
	const getAwsBuckets = async () => {
		const buckets = await getBuckets(endpoint, accessKey, secretKey);
		console.log(buckets);
	};
	// if(localStorage.getItem('endpoint')){
	// 	getAwsBuckets()
	// }
	const getItemsFromBucket = async (itemName: string) => {
		console.log(itemName);
		return 0;
	};
	const testdata = {
		Buckets: [
			{
				Name: 'demo-bucket',
				CreationDate: '2022-05-07T14:58:15.615Z'
			},
			{
				Name: 'hehe',
				CreationDate: '2022-09-01T15:31:54.214Z'
			},
			{
				Name: 'newb',
				CreationDate: '2022-09-14T17:49:54.091Z'
			},
			{
				Name: 'tezt',
				CreationDate: '2022-09-01T15:30:17.932Z'
			},
			{
				Name: 'waterbear',
				CreationDate: '2022-09-01T15:57:50.396Z'
			}
		],
		Owner: {
			DisplayName: 'minio',
			ID: '02d6176db174dc93cb1b899f7c6078f08654445fe8cf1b6ce98d8855f66bdbf4'
		}
	};
</script>

<Center>
	<Stack override={{ width: 500, margin: 50 }}>
		<Input placeholder="AWS Access key ID" bind:value={accessKey} />
		<Input placeholder="Endpoint URL" bind:value={endpoint} />
		<Input placeholder="AWS Secret Key" bind:value={secretKey} />

		<!-- <Button on:click={setLocalStorageVars}>Connect</Button> -->
		{#each testdata.Buckets as item}
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
	</Stack>
</Center>
