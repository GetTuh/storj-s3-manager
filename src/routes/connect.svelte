<script context="module" lang="ts">
	import AWS from 'aws-sdk';

	const awsConnection = (AWSAccessKeyId: string, AWSSecretKey: string, AWSEndpoint: any) => {
		AWS.config.update({
			accessKeyId: AWSAccessKeyId,
			secretAccessKey: AWSSecretKey
		});
		return new AWS.S3({ endpoint: AWSEndpoint });
	};
	const listBuckets = async (s3: any) => {
		return await s3.listBuckets().promise();
	};

	export const getBuckets = async (
		AWSEndpointString: string,
		AWSAccessKeyId: string,
		AWSSecretKey: string
	) => {
		const AWSEndpoint = new AWS.Endpoint(AWSEndpointString);
		return await listBuckets(awsConnection(AWSAccessKeyId, AWSSecretKey, AWSEndpoint));
	};
</script>
