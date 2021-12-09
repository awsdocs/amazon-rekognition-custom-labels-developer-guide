# Purposing datasets<a name="md-dataset-purpose"></a>

How you label the training and test datasets in your project determines the type of model that you create\. With Amazon Rekognition Custom Labels you can create models that do the following\.
+ [Find objects, scenes, and concepts](#md-dataset-purpose-classification)
+ [Find object locations](#md-dataset-purpose-localization)
+ [Find brand locations](#md-dataset-purpose-brands)

## Find objects, scenes, and concepts<a name="md-dataset-purpose-classification"></a>

The model classifies the objects, scenes, and concepts that are associated with an entire image\.

You can create two types of classification model, *image classification* and *multi\-label classification*\. For both types of classification model, the model finds one or more matching labels from the complete set of labels used for training\. The training and test datasets both require at least two labels\. 

### Image classification<a name="md-dataset-image-classification"></a>

 

The model classifies images as belonging to a set of predefined labels\. For example, you might want a model that determines if an image contains a living space\. The following image might have a *living\_space* image\-level label\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/living_space1.jpeg)

For this type of model, add a single image\-level label to each of the training and test dataset images\. For an example project, see [Image classification](getting-started.md#gs-image-classification-example)\.

### Multi\-label classification<a name="md-dataset-image-classification-multi-label"></a>

The model classifies images into multiple categories, such as the type of flower and whether it has leaves, or not\. For example, the following image might have *mediterranean\_spurge* and *no\_leaves* image level labels\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/mediterranean_spurge3.jpg)

For this type of model assign image\-level labels for each category to the training and test dataset images\. For an example project, see [Multi\-label image classification](getting-started.md#gs-multi-label-image-classification-example)\.

### Assigning image\-level labels<a name="w51aac20c17c19b7c11"></a>

If your images are stored in an Amazon S3 bucket, you can use [folder names](md-create-dataset-s3.md) to automatically add image\-level labels\. For more information, see [Amazon S3 bucket](md-create-dataset-s3.md)\. You can also add image\-level labels to images after you create a dataset, For more information, see [Assigning image\-level labels to an image](md-assign-image-level-labels.md)\. You can add new labels as you need them\. For more information, see [Managing labels](md-labels.md)\.

## Find object locations<a name="md-dataset-purpose-localization"></a>

To create a model that predicts the location of objects in your images, you define object location bounding boxes and labels for the images in your training and test datasets\. A bounding box is a box that tightly surrounds an object\. For example, the following image shows bounding boxes around an Amazon Echo and an Amazon Echo Dot\. Each bounding box has an assigned label \(*Amazon Echo* or *Amazon Echo Dot*\)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/echos.png)

To find object locations, your datasets needs at least one label\. During model training, a further label is automatically created that represents the area outside of the bounding boxes on an image\. 

### Assigning bounding boxes<a name="w51aac20c17c19b9b9"></a>

 When you create your dataset, you can include bounding box information for your images\. For example, you can import a SageMaker Ground Truth format [manifest file](md-create-manifest-file.md) that contains bounding boxes\. You can also add bounding boxes after you create a dataset\. For more information, see [Locating objects with bounding boxes](md-localize-objects.md)\. You can add new labels as you need them\. For more information, see [Managing labels](md-labels.md)\.

## Find brand locations<a name="md-dataset-purpose-brands"></a>

If you want to find the location of brands, such as logos and animated characters, you can use two different types of images for your training dataset images\. 
+  Images that are of the logo only\. Each image needs a single image\-level label that represents the logo name\. For example, the image\-level label for the following image could be *Lambda*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/lambda-logo.jpg)
+ Images that contain the logo in natural locations, such as a football game or an architectual diagram\. Each training image needs bounding boxes that surround each instance of the logo\. For example, the following image shows an architectural diagram with labeled bounding boxes surrounding the AWS Lambda and Amazon Pinpoint logos\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/brand-detection-lambda.png)

We recommend that you don't mix image\-level labels and bounding boxes in your training images\. 

The test images must have bounding boxes around instances of the brand that you want to find\. You can split the training dataset to create the test dataset, only if the training images include labeled bounding boxes\. If the training images only have image\-level labels, you must create a test dataset set that includes images with labeled bounding boxes\. If you train a model to find brand locations, do [Locating objects with bounding boxes](md-localize-objects.md) and [Assigning image\-level labels to an image](md-assign-image-level-labels.md) according to how you label your images\. 

The [Brand detection](getting-started.md#gs-brand-detection-example) example project shows how Amazon Rekognition Custom Labels uses labeled bounding boxes to train a model that finds object locations\.

## Label requirements for model types<a name="md-model-types-table"></a>

Use the following table to determine how to label your images\. 

You can combine image\-level labels and bounding box labeled images in a single dataset\. In this case, Amazon Rekognition Custom Labels chooses whether to create an image\-level model or an object location model\. 


| Example | Training images | Test images | 
| --- | --- | --- | 
|  [Image classification](#md-dataset-image-classification)  |  1 Image\-level label per image  |  1 Image\-level label per image   | 
|  [Multi\-label classification ](#md-dataset-image-classification-multi-label)  |  Multiple image\-level labels per image  |  Multiple image\-level labels per image  | 
|  [Find brand locations](#md-dataset-purpose-brands)  |  image level\-labels \(you can also use Labeled bounding boxes\)  |  Labeled bounding boxes  | 
|  [Find object locations](#md-dataset-purpose-localization)  |  Labeled bounding boxes  |  Labeled bounding boxes  | 