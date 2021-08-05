# Creating an Amazon Rekognition Custom Labels dataset<a name="cd-create-dataset"></a>

Datasets contain the images, labels, and bounding box information that is used to train and test an Amazon Rekognition Custom Labels model\. Datasets are managed by Amazon Rekognition Custom Labels projects\. You create the initial training dataset for a project during project creation\. You can also add new and existing datasets to a project after the project is created\. For example, you might create a dataset that is used to test a model\. 

You can create a dataset using images from one of the following locations\.
+ [Amazon S3 bucket](cd-s3.md)
+ [Local computer](cd-computer.md)
+ [Amazon SageMaker Ground Truth](cd-ground-truth.md)
+ [Existing dataset](cd-existing-dataset.md)

## Purposing datasets<a name="cd-dataset-purpose"></a>

We recommend that you create separate datasets for different image categories\. For example, if you have images of scenic views and images of dogs, you should create a dogs dataset and a scenic views dataset\.

### Finding objects, scenes, and concepts<a name="cd-classification"></a>

If you want to create a model that predicts the presence of objects, scenes, and concepts in your images, you assign image\-level labels to the images in your training and test datasets\. Each dataset requires at least two labels\. For example, the following image shows a scene\. Your dataset could include image\-level labels such *sunrise* or *countryside*\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/sunrise.png)

When you create your dataset, you assign image\-level labels to your images\. For example, if your images are stored in an Amazon S3 bucket, you can use [folder names](cd-s3.md) to add image\-level labels\. You can also add image\-level labels to images after you create a dataset\. For more information, see [Assigning image\-level labels to an image](rv-assign-labels.md)\. You can add new labels as you need them\. For more information, see [Labeling the dataset](rv-editing-labels.md)\.

The default number of images for a dataset that's used to find objects, scenes, and concepts is 250,000\. You can request an increase for up to 500,000 images per dataset\. For more information, [Service Quotas](https://docs.aws.amazon.com/general/latest/gr/rekognition_region.html#limits_rekognition)\.

### Finding object locations<a name="cd-localization"></a>

To create a model that predicts the location of objects in your images, you define object location bounding boxes and labels for the images in your training and test datasets\. A bounding box is a box that tightly surrounds an object\. For example, the following image shows bounding boxes around an Amazon Echo and an Amazon Echo Dot\. Each bounding box has an assigned label \(*Amazon Echo* or *Amazon Echo Dot*\)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/echos.png)

To find object locations, your datasets needs at least one label\. During model training, a further label is automatically created that represents the area outside of the bounding boxes on an image\. When you create your dataset, you can include bounding box information for your images\. For example, you can import a SageMaker Ground Truth format [manifest file](cd-manifest-files.md) that contains bounding boxes\. You can also add bounding boxes after you create a dataset\. For more information, see [Drawing bounding boxes](rv-bounding-box.md)\. You can add new labels as you need them\. For more information, see [Labeling the dataset](rv-editing-labels.md)\.

You can have up to 250,000 images in a dataset that's used to find object locations\. This value can't be increased\. For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\.

### Finding brands<a name="cd-brand-detection"></a>

If you want to find the location of brands, such as logos and animated characters, you can use two different types of images for your training dataset images\. 
+  Images that are of the logo only\. Each image needs a single image\-level label that represents the logo name\. For example, the image\-level label for the following image could be *Lambda*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/lambda-logo.jpg)
+ Images that contain the logo in natural locations, such as a football game or an architectual diagram\. Each training image needs bounding boxes that surround each instance of the logo\. For example, the following image shows an architectural diagram with labeled bounding boxes surrounding the AWS Lambda and Amazon Pinpoint logos\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/brand-detection-lambda.png)

We recommend that you don't mix image\-level labels and bounding boxes in your training images\. 

The test images must have bounding boxes around instances of the brand that you want to find\. 

### Combining image\-level and bounding box labeled images<a name="cd-combine"></a>

You can combine image\-level labels and bounding box labeled images in a single dataset\. In this case, Amazon Rekognition Custom Labels chooses whether to create an image\-level model or an object location model\. 

For information about adding labels to a dataset, see [Labeling the dataset](rv-editing-labels.md)\.