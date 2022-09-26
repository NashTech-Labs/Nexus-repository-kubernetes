## Setting up Sonatype Nexus Repository

1. Deploy the aws-efs-csi-driver helm chart
2. Create the EFS on AWS account and get the File system ID.
3. Create the S3 bucket for nexus blob storage.
4. Create the service account for S3 bucket access and configure serviceaccount it in values.yaml.
5. Update the bucket policy for the created S3 bucket. 
        {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "<role_arn>"
        },
        "Action": [
		"s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject",
                "s3:ListBucket",
                "s3:GetLifecycleConfiguration",
                "s3:PutLifecycleConfiguration",
                "s3:PutObjectTagging",
                "s3:GetObjectTagging",
                "s3:DeleteObjectTagging",
                "s3:GetBucketAcl"
		],        
        "Resource": [
          "arn:aws:s3:::<bucket_name>",
          "arn:aws:s3:::<bucket_name>/*"
      ]
    }
  ]
}
6. Configure the volumeid with file system id of efs per persistent storage and also configure the ingress in values.yaml.
7. Convert the nexus license into base64 and update the value in values.yaml for creating a secret and configure it under secret under data environment.
                base64 sonatype-license.lic

## Installing the Chart

To install the chart:

        helm install nexus-repository sonatype-nexus/ -f sonatype-nexus/values.yaml --namespace repository

The above command deploys Nexus on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

The default login is admin/admin123

## Uninstalling the Chart

To uninstall/delete the deployment:
        
        helm delete sonatype-nexus --namespace repository
