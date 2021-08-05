# Security<a name="sc-introduction"></a>

You can secure the management of your models and the `DetectCustomLabels` API that your customers use to detect custom labels\. 

For more information about securing Amazon Rekognition, see [Amazon Rekognition Security](https://docs.aws.amazon.com/rekognition/latest/dg/security.html)\.

## Securing Amazon Rekognition Custom Labels projects<a name="sc-resources"></a>

You can secure your Amazon Rekognition Custom Labels projects by specifying the resource\-level permissions that are specified in identity\-based policies\. For more information, see [Identity\-Based Policies and Resource\-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html)\. 

The Amazon Rekognition Custom Labels resources that you can secure are:


| Resource | Amazon Resource Name Format | 
| --- | --- | 
|  Project  |  arn:aws:rekognition:\*:\*:project/*project\_name*/datetime  | 
|  Model  |  arn:aws:rekognition:\*:\*:project/*project\_name*/version/*name*/datetime  | 

The following example policy shows how to give an identity permission to:
+ Describe all projects\.
+ Create, start, stop, and use a specific model for inference\.
+ Create a project\. Create and describe a specific model\.
+ Deny the creation of a specific project\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllResources",
            "Effect": "Allow",
            "Action": "rekognition:DescribeProjects",
            "Resource": "*"
        },
        {
            "Sid": "SpecificProjectVersion",
            "Effect": "Allow",
            "Action": [
                "rekognition:StopProjectVersion",
                "rekognition:StartProjectVersion",
                "rekognition:DetectCustomLabels",
                "rekognition:CreateProjectVersion"
            ],
            "Resource": "arn:aws:rekognition:*:*:project/MyProject/version/MyVersion/*"
       },
        {
            "Sid": "SpecificProject",
            "Effect": "Allow",
            "Action": [
                "rekognition:CreateProject",
                "rekognition:DescribeProjectVersions",
                "rekognition:CreateProjectVersion"
            ],
            "Resource": "arn:aws:rekognition:*:*:project/MyProject/*"
        },
        {
            "Sid": "ExplicitDenyCreateProject",
            "Effect": "Deny",
            "Action": [
                "rekognition:CreateProject"
            ],
            "Resource": ["arn:aws:rekognition:*:*:project/SampleProject/*"]
        }
    ]
}
```

## Securing DetectCustomLabels<a name="sc-detect-custom-labels"></a>

The identity used to detect custom labels might be different from the identity that manages Amazon Rekognition Custom Labels models\.

You can secure access an identity’s access to `DetectCustomLabels` by applying a policy to the identity\. The following example restricts access to `DetectCustomLabels` only and to a specific model\. The identity doesn’t have access to any of the other Amazon Rekognition operations\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "rekognition:DetectCustomLabels"


            ],
            "Resource": "arn:aws:rekognition:*:*:project/MyProject/version/MyVersion/*"
        }
    ]
}
```

## AWS managed policy: AmazonRekognitionCustomLabelsFullAccess<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

### Using AmazonRekognitionCustomLabelsFullAccess<a name="security-iam-awsmanpol-custom-labels-full-access"></a>

This policy is for Amazon Rekognition Custom Labels users\. Use the AmazonRekognitionCustomLabelsFullAccess policy to allow users full access to the Amazon Rekognition Custom Labels API and full access to the console buckets created by the Amazon Rekognition Custom Labels console\.  

**Permissions details**

This policy includes the following permissions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:ListAllMyBuckets",
                "s3:GetBucketAcl",
                "s3:GetBucketLocation",
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectTagging",
                "s3:GetObjectVersion",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::*custom-labels*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "rekognition:CreateProject",
                "rekognition:CreateProjectVersion",
                "rekognition:StartProjectVersion",
                "rekognition:StopProjectVersion",
                "rekognition:DescribeProjects",
                "rekognition:DescribeProjectVersions",
                "rekognition:DetectCustomLabels",
                "rekognition:DeleteProject",
                "rekognition:DeleteProjectVersion"
                "rekognition:TagResource",
                "rekognition:UntagResource",
                "rekognition:ListTagsForResource"
            ],
            "Resource": "*"
        }
    ]
}
```

## Amazon Rekognition Custom Labels updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Amazon Rekognition Custom Labels since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon Rekognition Custom Labels Document history page\.




| Change | Description | Date |  |  |  | 
| --- | --- | --- | --- | --- | --- | 
|  [AmazonRekognitionCustomLabelsFullAccess](https://docs.aws.amazon.com/rekognition/latest/customlabels-dg/sc-introduction.html#security-iam-awsmanpol) tagging update  |  Amazon Rekognition Custom Labels added new tagging actions\.  | April 2, 2021 | 
|  Amazon Rekognition Custom Labels started tracking changes  |  Amazon Rekognition Custom Labels started tracking changes for its AWS managed policies\.  | April 2, 2021 | 