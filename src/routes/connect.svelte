<script context="module" lang="ts">
	import AWS from 'aws-sdk';

	const awsConnection = (AWSAccessKeyId: string, AWSSecretKey: string, AWSEndpointString: any) => {
		AWS.config.update({
			accessKeyId: AWSAccessKeyId,
			secretAccessKey: AWSSecretKey
		});
		const AWSEndpoint = new AWS.Endpoint(AWSEndpointString);
		return new AWS.S3({ endpoint: AWSEndpoint });
	};

	export const getBuckets = async (
		AWSEndpointString: string,
		AWSAccessKeyId: string,
		AWSSecretKey: string
	) => {
		return await awsConnection(AWSAccessKeyId, AWSSecretKey, AWSEndpointString)
			.listBuckets()
			.promise();
	};
	export const getItemsFromBucket = async (
		AWSBucket: string,
		AWSEndpointString: string,
		AWSAccessKeyId: string,
		AWSSecretKey: string
	) => {
		return await awsConnection(AWSAccessKeyId, AWSSecretKey, AWSEndpointString)
			.listObjectsV2({ Bucket: AWSBucket })
			.promise();
	};
	export const deleteItem = async (
		AWSBucket: string,
		AWSEndpointString: string,
		AWSAccessKeyId: string,
		AWSSecretKey: string,
		AWSObjectKey: string
	) => {
		awsConnection(AWSAccessKeyId, AWSSecretKey, AWSEndpointString).deleteObject(
			{
				Bucket: AWSBucket,
				Key: AWSObjectKey
			},
			(err) =>err? alert(err):console.log('Item deleted')
		);
	};
	export const putObject = async (
		AWSBucket: string,
		AWSEndpointString: string,
		AWSAccessKeyId: string,
		AWSSecretKey: string,
		AWSObjectKey: string
	) => {
		awsConnection(AWSAccessKeyId, AWSSecretKey, AWSEndpointString).putObject(
			{
				Body: 'asd',
				Key: AWSObjectKey,
				Bucket: AWSBucket
			},
			(err) => alert(err)
		);
	};
</script>
