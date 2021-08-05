# Amazon SageMaker Ground Truth<a name="cd-ground-truth"></a>

With Amazon SageMaker Ground Truth, you can use workers from either Amazon Mechanical Turk, a vendor company that you choose, or an internal, private workforce along with machine learning to enable you to create a labeled set of images\. Amazon Rekognition Custom Labels imports SageMaker Ground Truth manifest files from an Amazon S3 bucket that you specify\.

Amazon Rekognition Custom Labels supports the following SageMaker Ground Truth tasks\.
+ [Image Classification](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-image-classification.html)
+ [Bounding Box](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-bounding-box.html)

The files you import are the images and a manifest file\. The manifest file contains label and bounding box information for the images you import\.

If your images and labels aren't in the format of a SageMaker Ground Truth manifest file, you can create a SageMaker format manifest file and use it to import your labeled images\. For more information, see [Creating a manifest file](cd-manifest-files.md)\.

Amazon Rekognition needs permissions to access the Amazon S3 bucket where your images are stored\. If you are using the console bucket set up for you by Amazon Rekognition Custom Labels, the required permissions are already set up\. If you are not using the console bucket, see [Accessing external Amazon S3 Buckets](su-console-policy.md#su-external-buckets)\.

The following procedure shows you how to create a dataset by using images labeled by a SageMaker Ground Truth job\. The job output files are stored in your Amazon Rekognition Custom Labels console bucket\.

<a name="create-dataset-procedure-ground-truth"></a>

**To create a dataset using images labeled by a SageMaker Ground Truth job \(console\)**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the console bucket, [create a folder](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-folder.html) to hold your images\. 
**Note**  
The console bucket is created when you first open the Amazon Rekognition Custom Labels console in an AWS Region\. For more information, see [Step 3: Create your project](tutorial-step-create-bucket.md)\.

1. [Upload your images](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) to the folder that you just created\.

1. In the console bucket, create a folder to hold the output of the Ground Truth job\.

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Create a Ground Truth labeling job\. You'll need the Amazon S3 URLs for the folders you created in step 2 and step 4\. For more information, see [Use Amazon SageMaker Ground Truth for Data Labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms.html)\. 

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Datasets**\.

1. In the **Datasets** section, choose **Create dataset**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/project-resources.png)

1. In the **Create dataset** details page, enter a name for your dataset\.

1. For **Image location** choose **Import images labeled by SageMaker Ground Truth**\.

1. In **\.manifest file location** enter the location of the `output.manifest` file in the folder you created in step 4\. It should be in the sub\-folder `Ground-Truth-Job-Name/manifests/output`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/create-dataset-ground-truth-import.png)

1. Choose **Submit**\. 