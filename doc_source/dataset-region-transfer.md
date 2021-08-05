# Copying a dataset to a different AWS region<a name="dataset-region-transfer"></a>

You might want to use an existing Amazon Rekognition Custom Labels dataset in a different AWS Region, such as a new AWS Region that Amazon Rekognition Custom Labels supports\.

The following instructions show you how to copy a dataset \(images and manifest file\) from the console bucket of a source Region, to the console bucket of a target Region\.

**To copy a dataset \(console\)**

1. Get the Amazon S3 bucket of the dataset in the source Region\.

   1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

   1. Choose **Use Custom Labels**\.

   1. At the top right, choose **Select a Region** and choose the region that contains the source dataset\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/region.png)

   1. \(Optional\) If you need to find the datasets used to train a specific model, do the following:

      1. In the navigation pane, choose **Projects**

      1. Choose the project that contains the desired model\.

      1. Note the training and test datasets for the desired model\.

   1. In the navigation pane, choose **Datasets**\. 

   1. In the **Location** column, choose the **S3 bucket** link for the source dataset\.

   1. In the Amazon S3 console, note the Amazon S3 bucket name\. In an S3 URI, The bucket name is immediatly after `s3://`\. For example, `s3://custom-labels-console-us-east-1-1234567891`\.

1. Get the name of the console bucket used by Amazon Rekognition in the target Region\. Do the procedure that matches your Custom Labels usage\.

------
#### [ Custom Labels already used in target Region  ]

   1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

   1. Note the name of the console bucket that matches the target Region\. For example, `custom-labels-console-us-west-2-1234567891`\.
**Note**  
Console bucket names are preceded with *custom\-labels\-console\-*\.

------
#### [ Custom Labels not used in target Region ]

   1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

   1. Choose **Use Custom Labels**\.

   1. Choose **Select a region** and choose the region that you want to copy the dataset to\. 

   1. Do the following using the **First Time Set Up** dialog box:

      1. Copy down the name of the Amazon S3 bucket that's shown\. You'll need this information later\.

      1. Choose **Create S3 bucket** to let Amazon Rekognition Custom Labels create an Amazon S3 bucket \(console bucket\) on your behalf\.

      1. \(Optional\) continue to create a project\. You can use the project to manage your target dataset\. For more information, see [Managing an Amazon Rekognition Custom Labels Project](cp-manage-project.md)\.

------

1. Use the following Python code to update and copy the manifest file to the target region\. Change the following values:
   + `source_bucket` to the name of the bucket you identified in step 1\.
   + `target_bucket` to the name of the bucket you identified in step 2\.
   + `dataset_name` to the name of the source dataset\.

   ```
   #!/usr/bin/env python
   # coding: utf-8
   
   import boto3
   import botocore
   import logging
   
   # this is the S3 Source Bucket Name
   source_bucket = 'source_bucket'
   
   # this is the S3 Target Bucket Name
   target_bucket = 'target_bucket'
   
   # This is the S3 bucket key
   bucket_key = 'datasets/dataset_name/manifests/output/output.manifest'
   
   # Step 1: Download the manifest file from the source bucket
   s3 = boto3.resource('s3')
   try:
       s3.Bucket(source_bucket).download_file(bucket_key, 'original.manifest')
   except botocore.exceptions.ClientError as e:
       if e.response['Error']['Code'] == "404":
           print("The object does not exist.")
       else:
           raise
   # Step 2: Replace Source-ref URL from Source to Target Bucket Name
   
   fin = open('original.manifest', 'rt')
   fout = open('output.manifest', 'wt')
   
   for line in fin:
       fout.write(line.replace(source_bucket,target_bucket))
   
   fin.close()
   fout.close()
   
   # Step 3: Upload the new manifest file to the target bucket.
   s3_client = boto3.client('s3')
   try:
       response = s3_client.upload_file('output.manifest', target_bucket, bucket_key)
   except botocore.exceptions.ClientError as e:
       logging.error(e)
   ```

1. Open the Amazon Rekognition console in the target Region\. 

1. From the navigation pane, choose **Datasets** and confirm that the dataset has been created\.

1. Use the the dataset to create a model in the new Region\. For more information, see [Training an Amazon Rekognition Custom Labels model](tm-train-model.md)\.