# Local computer<a name="md-create-dataset-computer"></a>

The images are loaded directly from your computer\. You can upload up to 30 images at a time\.

The images you upload won't have labels associated with them\. For more information, see [Labeling images](md-labeling-images.md)\. 

**Note**  
If you have many images to upload, consider using an Amazon S3 bucket\. For more information, see [Amazon S3 bucket](md-create-dataset-s3.md)\.

**To create a dataset using images on a local computer \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project to which you want to add a dataset\. The details page for your project is displayed\.

1. Choose **Create dataset**\. The **Create dataset** page is shown\.

1. In **Starting configuration**, choose either **Start with a single dataset** or **Start with a training dataset**\. To create a higher quality model, we recommend starting with separate training and test datasets\.

------
#### [ Single dataset ]

   1. In the **Training dataset details section** section, choose **Upload images from your computer**\.

   1. Choose **Create Dataset**\. 

   1. On the project's dataset page, choose **Add images**\. 

   1. Choose the images you want to upload into the dataset from your computer files\. You can drag the images or choose the images that you want to upload from your local computer\.

   1. Choose **Upload images**\.

------
#### [ Separate training and test datasets ]

   1. In the **Training dataset details** section, choose **Upload images from your computer**\.

   1. In the **Test dataset details** section, choose **Upload images from your computer**\.
**Note**  
Your training and test datasets can have different image sources\.

   1. Choose **Create Datasets**\. Your project's datasets page appears with a **Training** tab and a **Test** tab for the respective datasets\. 

   1. Choose **Actions** and then choose **Add images to training dataset**\.

   1. Choose the images you want to upload to the dataset\. You can drag the images or choose the images that you want to upload from your local computer\.

   1. Choose **Upload images**\.

   1. Repeat steps 5e \- 5g\. For step 5e, choose **Actions** and then choose **Add images to test dataset**\.

------

1. Follow the steps in [Labeling images](md-labeling-images.md) to label your images\.

1. Follow the steps in [Training a model \(Console\)](training-model.md#tm-console) to train your model\.