# Creating an Amazon Rekognition Custom Labels Dataset<a name="cd-create-dataset"></a>

Datasets contain the images, labels, and bounding box information that is used to train and test an Amazon Rekognition Custom Labels model\. Datasets are managed by Amazon Rekognition Custom Labels projects\. You create the initial training dataset for a project during project creation\. You can also add new and existing datasets to a project after the project is created\. For example, you might create a dataset that is used to test a model\. 

We recommend that you create separate datasets for different image categories\. For example, if you have images of scenic views and images of dogs, you should create a dogs dataset and a scenic views dataset\.

You can create a dataset using images imported from one of the following locations:
+ S3 bucket – The images are imported from an Amazon S3 bucket\. Folder names can be used to determine the labels for images within top\-level folders\. Child folders aren't used to generate labels\. If the Amazon S3 bucket that you are using to import images is different \(external\) to the bucket created by the console, the console prompts you to set up appropriate permissions\. For more information, see [Step 5: Set Up Amazon Rekognition Custom Labels Console Permissions](su-console-policy.md)\. 
+ Your computer – The images are loaded directly from your computer\. You can upload up to 30 images at a time\. You need to add labels and bounding box information\.  
+ Amazon SageMaker Ground Truth – With Amazon SageMaker Ground Truth, you can use workers from either Amazon Mechanical Turk, a vendor company that you choose, or an internal, private workforce along with machine learning to enable you to create a labeled set of images\. Amazon Rekognition Custom Labels imports Amazon SageMaker Ground Truth manifest files from an Amazon S3 bucket that you specify\.

  The files you import are the images and a manifest file\. The manifest file contains label and bounding box information for the images you import\.

  If your images and labels aren't in the format of an Amazon SageMaker Ground Truth manifest file, you can create a manifest file and use it to import your labeled images\. For more information, see [Manifest Files](cd-manifest-files.md)\.
+ An existing dataset – If you've previously created a dataset, you can copy the dataset contents to a new dataset\.

After you have imported your images, you can add more images, assign labels, and add bounding boxes from a dataset's gallery page\. For more information, see [Review Your Images](cd-managing-datasets.md#rv-images)\.

You choose which datasets to use for training and testing when you start training\. For more information, see [Training an Amazon Rekognition Custom Labels Model](tm-train-model.md)\. <a name="create-dataset-procedure"></a>

**To create a dataset \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project to which you want to add a dataset\.

1. In the **Datasets** section of the project page, choose **Create dataset**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/project-resources.png)

1. In the **Create dataset** details page, enter a name for your dataset\.

1. Choose the import location for your dataset and follow the instructions\.

1. Choose **Submit**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/create-dataset.png)