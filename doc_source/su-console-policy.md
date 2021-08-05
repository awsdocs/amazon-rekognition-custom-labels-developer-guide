# Step 4: Set up Amazon Rekognition Custom Labels permissions<a name="su-console-policy"></a>

To use the Amazon Rekognition you need add to have appropriate permissions\. If you want to store your training files in a bucket other than the console bucket, you need additional permissions\.

**Topics**
+ [Allowing console access](#su-console-access)
+ [Accessing external Amazon S3 Buckets](#su-external-buckets)
+ [Policy updates for using the AWS SDK](#su-sdk-policy-update)

## Allowing console access<a name="su-console-access"></a>

The Identity and Access Management \(IAM\) user or group that uses the Amazon Rekognition Custom Labels consoles needs the following IAM policy that covers Amazon S3, SageMaker Ground Truth, and Amazon Rekognition Custom Labels\. To allow console access, use the following IAM policy\. For information about adding IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)\.



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "*"
        },
        {
            "Sid": "s3Policies",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:CreateBucket",
                "s3:GetBucketAcl",
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectVersion",
                "s3:GetObjectTagging",
                "s3:GetBucketVersioning",
                "s3:GetObjectVersionTagging",
                "s3:PutBucketCORS",
                "s3:PutLifecycleConfiguration",
                "s3:PutBucketPolicy",
                "s3:PutObject",
                "s3:PutObjectTagging",
                "s3:PutBucketVersioning",
                "s3:PutObjectVersionTagging"
            ],
            "Resource": [
                "arn:aws:s3:::custom-labels-console-*",
                "arn:aws:s3:::rekognition-video-console-*"
            ]
        },
        {
            "Sid": "rekognitionPolicies",
            "Effect": "Allow",
            "Action": [
                "rekognition:*"
            ],
            "Resource": "*"
        },
        {
            "Sid": "groundTruthPolicies",
            "Effect": "Allow",
            "Action": [
                "groundtruthlabeling:*"
            ],
            "Resource": "*"
        }
    ]
}
```

## Accessing external Amazon S3 Buckets<a name="su-external-buckets"></a>

When you first open the Amazon Rekognition Custom Labels console in a new AWS Region, Amazon Rekognition Custom Labels creates a bucket \(console bucket\) that's used to store project files\. Alternatively, you can use your own Amazon S3 bucket \(external bucket\) to upload the images or manifest file to the console\. To use an external bucket, add the following policy block to the preceding policy\. Replace `my-bucket` with the name of the bucket\.

```
{
    "Sid": "s3ExternalBucketPolicies",
    "Effect": "Allow",
    "Action": [
        "s3:GetBucketAcl",
        "s3:GetBucketLocation",
        "s3:GetObject",
        "s3:GetObjectAcl",
        "s3:GetObjectVersion",
        "s3:GetObjectTagging",
        "s3:ListBucket",
        "s3:PutObject"
    ],
    "Resource": [
        "arn:aws:s3:::my-bucket*"
    ]
}
```

## Policy updates for using the AWS SDK<a name="su-sdk-policy-update"></a>

To use the AWS SDK with the latest release of Amazon Rekognition Custom Labels, you no longer need to give Amazon Rekognition Custom Labels permissions to access the Amazon S3 bucket that contains your training and testing images\. If you have previously added permissions, You don't need to remove them\. If you choose to, remove any policy from the bucket where the service for the principal is `rekognition.amazonaws.com`\. For example:

```
"Principal": {
    "Service": "rekognition.amazonaws.com"
}
```

For more information, see [Using bucket policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html)