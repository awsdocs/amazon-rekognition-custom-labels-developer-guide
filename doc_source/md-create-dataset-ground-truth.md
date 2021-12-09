# Amazon SageMaker Ground Truth<a name="md-create-dataset-ground-truth"></a>

You can create a dataset using an Amazon SageMaker Ground Truth format manifest file\. You can use the manifest file from an Amazon SageMaker Ground Truth job, or you can create your own manifest file\. To create your own manifest file, see [Creating a manifest file](md-create-manifest-file.md)\.

With Amazon SageMaker Ground Truth, you can use workers from either Amazon Mechanical Turk, a vendor company that you choose, or an internal, private workforce along with machine learning that allows you to create a labeled set of images\. Amazon Rekognition Custom Labels imports SageMaker Ground Truth manifest files from an Amazon S3 bucket that you specify\.

Amazon Rekognition Custom Labels supports the following SageMaker Ground Truth tasks\.
+ [Image Classification](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-image-classification.html)
+ [Bounding Box](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-bounding-box.html)

The files you import are the images and a manifest file\. The manifest file contains label and bounding box information for the images you import\.

If your images and labels aren't in the format of a SageMaker Ground Truth manifest file, you can create a SageMaker format manifest file and use it to import your labeled images\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.

Amazon Rekognition needs permissions to access the Amazon S3 bucket where your images are stored\. If you are using the console bucket set up for you by Amazon Rekognition Custom Labels, the required permissions are already set up\. If you are not using the console bucket, see [Accessing external Amazon S3 Buckets](su-console-policy.md#su-external-buckets)\.

**Topics**
+ [Creating a dataset with a SageMaker Groundtruth job \(Console\)](#md-create-dataset-ground-truth-console)
+ [External](#creating-dataset-external-manifest-file)

## Creating a dataset with a SageMaker Groundtruth job \(Console\)<a name="md-create-dataset-ground-truth-console"></a>

The following procedure shows you how to create a dataset by using images labeled by a SageMaker Ground Truth job\. The job output files are stored in your Amazon Rekognition Custom Labels console bucket\.<a name="create-dataset-procedure-ground-truth"></a>

**To create a dataset using images labeled by a SageMaker Ground Truth job \(console\)**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the console bucket, [create a folder](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-folder.html) to hold your training images\. 
**Note**  
The console bucket is created when you first open the Amazon Rekognition Custom Labels console in an AWS Region\. For more information, see [Managing an Amazon Rekognition Custom Labels project](managing-project.md)\.

1. [Upload your images](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) to the folder that you just created\.

1. In the console bucket, create a folder to hold the output of the Ground Truth job\.

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. Create a Ground Truth labeling job\. You'll need the Amazon S3 URLs for the folders you created in step 2 and step 4\. For more information, see [Use Amazon SageMaker Ground Truth for Data Labeling](https://docs.aws.amazon.com/sagemaker/latest/dg/sms.html)\. 

1. Repeat steps 1 \- 6 to create SageMaker Ground Truth job for your test dataset\.

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project to which you want to add a dataset\. The details page for your project is displayed\.

1. Choose **Create dataset**\. The **Create dataset** page is shown\.

1. In **Starting configuration**, choose either **Start with a single dataset** or **Start with a training dataset**\. To create a higher quality model, we recommend starting with separate training and test datasets\.

------
#### [ Single dataset ]

   1. In the **Training dataset details** section, choose **Import images labeled by SageMaker Ground Truth**\.

   1. In **\.manifest file location** enter the location of the `output.manifest` file in the folder you created in step 4\. It should be in the sub\-folder `Ground-Truth-Job-Name/manifests/output`\.

   1. Choose **Create Dataset**\. The datasets page for your project opens\.

------
#### [ Separate training and test datasets ]

   1. In the **Training dataset details** section, choose **Import images labeled by SageMaker Ground Truth**\.

   1. In **\.manifest file location** enter the location of the `output.manifest` file in the folder you created in step 4\. It should be in the sub\-folder `Ground-Truth-Job-Name/manifests/output`\.

   1. In the **Test dataset details** section, choose **Import images labeled by SageMaker Ground Truth**\.
**Note**  
Your training and test datasets can have different image sources\.

   1. In **\.manifest file location** enter the location of the `output.manifest` file in the folder you created for the test images \(step 7\)\. It should be in the sub\-folder `Ground-Truth-Job-Name/manifests/output`\.

   1. Choose **Create Datasets**\. The datasets page for your project opens\.

------

1. If you need to add or change labels, do [Labeling images](md-labeling-images.md)\.

1. Follow the steps in [Training a model \(Console\)](training-model.md#tm-console) to train your model\.

## External<a name="creating-dataset-external-manifest-file"></a>