# Amazon S3 bucket<a name="md-create-dataset-s3"></a>

The images are imported from an Amazon S3 bucket\. You can use the console bucket, or another Amazon S3 bucket in your AWS account\. If you are using the console bucket, the required permissions are already set up\. If you are not using the console bucket, see [Accessing external Amazon S3 Buckets](su-console-policy.md#su-external-buckets)\.

During dataset creation, you can choose to assign label names to images based on the name of the folder that contains the images\. The folder\(s\) must be a child of the Amazon S3 folder path that you specify in **S3 folder location** during dataset creation\. To create a dataset, see [Creating a dataset by importing images from an S3 bucket](#cd-procedure)\.

For example, assume the following folder structure in an Amazon S3 bucket\. If you specify the Amazon S3 folder location as *S3\-bucket/alexa\-devices*, the images in the folder *echo* are assigned the label *echo*\. Similarly, images in the folder *echo\-dot* are assigned the label *echo\-dot*\. The names of deeper child folders aren't used to label images\. Instead, the appropriate child folder of the Amazon S3 folder location is used\. For example, images in the folder *white\-echo\-dots* are assigned the label *echo\-dot*\. Images at the level of the S3 folder location \(*alexa\-devices*\) don't have labels assigned to them\.

 Folders deeper in the folder structure can be used to label images by specifying a deeper S3 folder location\. For example, If you specify *S3\-bucket/alexa\-devices/echo\-dot*, Images in the folder *white\-echo\-dot* are labeled *white\-echo\-dot*\. Images outside the specified s3 folder location, such as *echo*, aren't imported\.

```
S3-bucket
└── alexa-devices
    ├── echo
    │   ├── echo-image-1.png
    │   └── echo-image-2.png
    │   ├── .
    │   └── .
    └── echo-dot
        ├── white-echo-dot
        │   ├── white-echo-dot-image-1.png
        │   ├── white-echo-dot-image-2.png
        │
        ├── echo-dot-image-1.png
        ├── echo-dot-image-2.png
        ├── .
        └── .
```

We recommend that you use the Amazon S3 bucket \(console bucket\) created for you by Amazon Rekognition when you first opened the console in the current AWS region\. If the Amazon S3 bucket that you are using is different \(external\) to the console bucket, the console prompts you to set up appropriate permissions during dataset creation\. For more information, see [Step 4: Set up Amazon Rekognition Custom Labels permissions](su-console-policy.md)\. 

## Creating a dataset by importing images from an S3 bucket<a name="cd-procedure"></a>

The following procedure shows you how to create a dataset using images stored in the Console S3 bucket\. The images are automatically labeled with the name of the folder in which they are stored\. 

After you have imported your images, you can add more images, assign labels, and add bounding boxes from a dataset's gallery page\. For more information, see [Labeling images](md-labeling-images.md)\.<a name="cd-upload-s3-bucket"></a>

**Upload your images to an Amazon Simple Storage Service bucket**

1. Create a folder on your local file system\. Use a folder name such as *alexa\-devices*\.

1. Within the folder you just created, create folders named after each label that you want to use\. For example, *echo* and *echo\-dot*\. The folder structure should be similar to the following\.

   ```
   alexa-devices
   ├── echo
   │   ├── echo-image-1.png
   │   ├── echo-image-2.png
   │   ├── .
   │   └── .
   └── echo-dot
       ├── echo-dot-image-1.png
       ├── echo-dot-image-2.png
       ├── .
       └── .
   ```

1. Place the images that correspond to a label into the folder with the same label name\.

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. [Add the folder](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/upload-objects.html) you created in step 1 to the Amazon S3 bucket \(console bucket\) created for you by Amazon Rekognition Custom Labels during *First Time Set Up*\. For more information, see [Managing an Amazon Rekognition Custom Labels project](managing-project.md)\.

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project to which you want to add a dataset\. The details page for your project is displayed\.

1. Choose **Create dataset**\. The **Create dataset** page is shown\.

1. In **Starting configuration**, choose either **Start with a single dataset** or **Start with a training dataset**\. To create a higher quality model, we recommend starting with separate training and test datasets\.

------
#### [ Single dataset ]

   1. In the **Training dataset details** section, choose **Import images from S3 bucket**\.

   1. In the **Training dataset details** section, Enter the information for steps 13 \- 15 in the **Image source configuration** section\. 

------
#### [ Separate training and test datasets ]

   1. In the **Training dataset details** section, choose **Import images from S3 bucket**\.

   1. In the **Training dataset details** section, enter the information for steps 13 \- 15 in the **Image source configuration** section\. 

   1. In the **Test dataset details** section, choose **Import images from S3 bucket**\.

   1. In the **Test dataset details** section, enter the information for steps 13 \- 15 in the **Image source configuration** section\. 

------

1. Choose **Import images from Amazon S3 bucket**\.

1. In **S3 URI**, enter the Amazon S3 bucket location and folder path\. 

1. Choose **Automatically attach labels to images based on the folder**\.

1. Choose **Create Datasets**\. The datasets page for your project opens\.

1. If you need to add or change labels, do [Labeling images](md-labeling-images.md)\.

1. Follow the steps in [Training a model \(Console\)](training-model.md#tm-console) to train your model\.