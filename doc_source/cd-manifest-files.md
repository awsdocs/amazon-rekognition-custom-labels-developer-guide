# Creating a Manifest File<a name="cd-manifest-files"></a>

You can create a test or training dataset by importing an SageMaker Ground Truth format manifest file\. If your images are labeled in a format that isn't an SageMaker Ground Truth manifest file, use the following information to create an SageMaker Ground Truth format manifest file\. 

Manifest files are in [JSON lines](http://jsonlines.org) format where each line is a complete JSON object representing the labeling information for an image\. Amazon Rekognition Custom Labels supports SageMaker Ground Truth manifests with JSON lines in the following formats:
+ [Classification Job Output](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-data-output.html#sms-output-class) – Use to add image\-level labels to an image\. An image\-level label defines the class of scene, concept, or object \(if object location information isn't needed\) that's on an image\. An image can have more that one image\-level label\. For more information, see [Image\-Level Labels in Manifest Files](cd-manifest-files-classification.md)\.
+ [Bounding Box Job Output](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-data-output.html#sms-output-box) – Use to label the class and location of one or more objects on an image\. For more information, see [Object Localization in Manifest Files](cd-manifest-files-object-detection.md)\.

Image\-level and localization \(bounding\-box\) JSON lines can be chained together in the same manifest file\. 

**Note**  
The JSON line examples in this section are formatted for readability\. 

When you import a manifest file, Amazon Rekognition Custom Labels applies validation rules for limits, syntax, and semantics\. For more information, see [Validation Rules for Manifest Files](cd-manifest-files-validation-rules.md)\. 

The images referenced by a manifest file must be located in the same Amazon S3 bucket as the manifest file\. You specify the location of an image in the `source-ref` field of a JSON line\. 

Amazon Rekognition needs permissions to access the Amazon S3 bucket where your images are stored\. If you are using the Console Bucket set up for you by Amazon Rekognition Custom Labels, the required permissions are already set up\. If you are not using the console bucket, see [External Amazon S3 Buckets](su-console-policy.md#su-external-buckets)\.

The following procedure shows you how to create a dataset by importing an SageMaker format manifest file that is stored in the console bucket\.

<a name="create-dataset-procedure-manifest-file"></a>

**To create a dataset using an SageMaker Ground Truth format manifest file \(console\)**

1. Create your SageMaker Ground Truth format manifest file\. For more information, see [Image\-Level Labels in Manifest Files](cd-manifest-files-classification.md) and [Object Localization in Manifest Files](cd-manifest-files-object-detection.md)\.

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the Console Bucket, [create a folder](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-folder.html) to hold your manifest file\. 
**Note**  
The console bucket is created when you first open the Amazon Rekognition Custom Labels console in an AWS Region\. For more information, see [Step 2: Create Your First Project](gs-step-create-bucket.md)\.

1. [Upload your manifest file](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) to the folder that you just created\.

1. In the console bucket, create a folder to hold your images\.

1. Upload your images to the folder you just created\.
**Important**  
The `source-ref` field value in each JSON line must map to images in the folder\. 

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Datasets**\.

1. In the **Datasets** section, choose **Create dataset**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/project-resources.png)

1. In the **Create dataset** details page, enter a name for your dataset\.

1. For **Image location**, choose **Import images labeled by SageMaker Ground Truth**\.

1. For **\.manifest file location**, enter the location of your manifest file\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/create-dataset-ground-truth-import.png)

1. Choose **Submit**\. 

Amazon Rekognition Custom Labels creates a dataset in the console bucket `datasets` folder\. Your original `.manifest` file remains unchanged\.

**Topics**
+ [Image\-Level Labels in Manifest Files](cd-manifest-files-classification.md)
+ [Object Localization in Manifest Files](cd-manifest-files-object-detection.md)
+ [Validation Rules for Manifest Files](cd-manifest-files-validation-rules.md)
+ [Transforming COCO Datasets](cd-transform-coco.md)