<script context='module' lang='ts'>

import AWS from 'aws-sdk';
import json from './../../token.json';

const awsConnection=(AWSAccessKeyId:string,AWSSecretKey:string,AWSEndpoint:any)=> {
	AWS.config.update({
		accessKeyId: AWSAccessKeyId,
		secretAccessKey: AWSSecretKey
	});
	return new AWS.S3({ endpoint: AWSEndpoint });
}
const listBuckets = (s3: any) => {
	console.log('getting')
	s3.listBuckets(function (error: any, data: any) {
		if (error != null) {
			console.log('Failed to retrieve an object: ' + error);
		} else {
			console.log('Loaded ' + data.ContentLength + ' bytes');
			return data
			// do something with data.Body
		}
	});
};

export const getBuckets=()=>{
	const AWSEndpoint = new AWS.Endpoint(json.AWSEndpoint);
	return listBuckets(awsConnection(json.AWSAccessKeyId, json.AWSSecretKey, AWSEndpoint));

}
</script>