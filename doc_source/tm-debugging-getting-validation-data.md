# Getting the validation results<a name="tm-debugging-getting-validation-data"></a>

The validation results contain error information for [Terminal manifest content errors](tm-debugging.md#tm-error-category-combined-terminal) and [Non terminal JSON line validation errors](tm-debugging.md#tm-error-category-non-terminal-errors)\. There are three validation results files\.
+ *training\_manifest\_with\_validation\.json* – A copy of the training dataset manifest file with JSON Line error information added\.
+ *testing\_manifest\_with\_validation\.json* – A copy of the testing dataset manifest file with JSON Line error error information added\. 
+ *manifest\_summary\.json* – A summary of manifest content errors and JSON Line errors found in the training and testing datasets\. For more information, see [Understanding the manifest summary](tm-debugging-summary.md)\.

For information about the contents of the training and testing validation manifests, see [Debugging a failed model training](tm-debugging.md)\. 

**Note**  
The validation results are created only if no [Terminal manifest file errors](tm-debugging.md#tm-error-category-terminal) are generated during training\.
If a [service error](tm-debugging.md#tm-error-category-service) occurs after the training and testing manifest are validated, the validation results are created, but the response from [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions) doesn't include the validation results file locations\.

After training completes or fails, you can download the validation results by using the Amazon Rekognition Custom Labels console or get the Amazon S3 bucket location by calling [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions) API\.

## Getting validation results \(Console\)<a name="tm-debugging-getting-validation-data-console"></a>

If you are using the console to train your model, you can download the validation results from a project's list of models, as shown in the following diagram\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/models-validation-results.jpg)

You can also access download the validation results from a model's details page\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/model-validation-results.jpg)

For more information, see [Training a model \(Console\)](training-model.md#tm-console)\. 

## Getting validation results \(SDK\)<a name="tm-debugging-getting-validation-data-sdk"></a>

After model training completes, Amazon Rekognition Custom Labels stores the validation results in the Amazon S3 bucket specified during training\. You can get the S3 bucket location by calling the [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions) API, after training completes\. To train a model, see [Training a model \(SDK\)](training-model.md#tm-sdk)\.

A [ValidationData](https://docs.aws.amazon.com/rekognition/latest/dg/API_ValidationData) object is returned for the training dataset \([TrainingDataResult](https://docs.aws.amazon.com/rekognition/latest/dg/API_TrainingDataResult)\) and the testing dataset \([TestingDataResult](https://docs.aws.amazon.com/rekognition/latest/dg/API_TestingDataResult)\)\. The manifest summary is returned in `ManifestSummary`\.

After you get the Amazon S3 bucket location, you can download the validation results\. For more information, see [How do I download an object from an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/download-objects.html)\. You can also use the [GetObject](https://docs.aws.amazon.com/AmazonS3/latest/dev/GettingObjectsUsingAPIs.html) operation\.

**To get validation data \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` and permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example to get the location of the validation results\. 

------
#### [ Python ]

   Replace `project_arn` with the Amazon Resource Name \(ARN\) of the project that contains the model\. For more information, see [Managing an Amazon Rekognition Custom Labels project](managing-project.md)\. Replace `version_name` with the name of the model version\. For more information, see [Training a model \(SDK\)](training-model.md#tm-sdk)\. 

   ```
   import boto3
   import io
   from io import BytesIO
   import sys
   import json
   
   
   def describe_model(project_arn, version_name):
   
       client=boto3.client('rekognition')
       
       response=client.describe_project_versions(ProjectArn=project_arn,
           VersionNames=[version_name])
   
       for model in response['ProjectVersionDescriptions']:
           print(json.dumps(model,indent=4,default=str))
          
   def main():
   
       project_arn='project_arn'
       version_name='version_name'
   
       describe_model(project_arn, version_name)
   
   if __name__ == "__main__":
       main()
   ```

------

1. In the program output, note the `Validation` field within the `TestingDataResult` and `TrainingDataResult` objects\. The manifest summary is in `ManifestSummary`\.