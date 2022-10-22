<script context="module" lang="ts">
	import AWS from 'aws-sdk';

	const awsConnection = (AWSAccessKeyId: string, AWSSecretKey: string, AWSEndpoint: any) => {
		AWS.config.update({
			accessKeyId: AWSAccessKeyId,
			secretAccessKey: AWSSecretKey
		});
		return new AWS.S3({ endpoint: AWSEndpoint });
	};

	export const getBuckets = async (
		AWSEndpointString: string,
		AWSAccessKeyId: string,
		AWSSecretKey: string
	) => {
		const AWSEndpoint = new AWS.Endpoint(AWSEndpointString);
		return await awsConnection(AWSAccessKeyId, AWSSecretKey, AWSEndpoint).listBuckets().promise();
	};
	export const getItemsFromBucket = async (
		AWSBucket: string,
		AWSEndpointString: string,
		AWSAccessKeyId: string,
		AWSSecretKey: string
	) => {
		const AWSEndpoint = new AWS.Endpoint(AWSEndpointString);
		let elo = await awsConnection(AWSAccessKeyId, AWSSecretKey, AWSEndpoint)
			.listObjectsV2({Bucket: AWSBucket})
			.promise();
		return elo;
	};
</script>
