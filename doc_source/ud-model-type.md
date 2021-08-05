# Step 1: Decide your model type<a name="ud-model-type"></a>

Decide which type of model you want to train\. You can train models that do the following\.
+ [Find objects, scenes, and concepts](#ud-classification)
+ [Find object locations](#ud-object-localization)
+ [Find the location of brands](#ud-brand-detection-localization)

To help you decide, Amazon Rekognition Custom Labels provides example projects that you can use\. For more information, see [Getting started with Amazon Rekognition Custom Labels](gs-introduction.md)\. 

 

Amazon Rekognition Custom Labels uses training images and test images to train a model\. The type of model that Amazon Rekognition Custom Labels trains depends on how you label your training and test images\. A label identifies an object, scene, or concept that's unique to your requirements\. You can assign a label to an entire image with an *image\-level* label, or you can assign a label to a bounding box that surrounds an object in an image\.

You use datasets to manage your images and the labels assigned to each image\. You create a training dataset and a test datasets later in this tutorial\. 

In this tutorial, you choose which type of model to train\.

## Find objects, scenes, and concepts<a name="ud-classification"></a>

The model predicts objects, scenes, and concepts associated with an entire image\. You use image\-level labels to assign the object, scene or concept to an image\.

To train a model to find objects, scenes, and concepts, your training and dataset images both need image\-level labels\. 

You can assign one or more image\-level labels to an image\. For example, you might want a model that determines if an image contains a tourist attraction, or not\. The single label for the following image would be *tourist attraction*\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/pateros.jpg)

If you want a model that categorizes images into multiple categories, assign image\-level labels for each category that the image represents\. For example, the previous image might have labels such as *blue sky*, *reflection*, and *lake*\.

If you use one or multiple image\-level labels, the model finds one or more matching labels from the complete set of labels used for training\.

The [Image classification](gs-introduction.md#gs-image-classification-example) and [Multi\-label image classification](gs-introduction.md#gs-multi-label-image-classification-example) example projects show different uses for image\-level labels\.

If you train a model that finds objects, scenes, and concepts, you don't need to do [Step 7: Locate objects with bounding boxes](tutorial-draw-bounding-boxes.md) as there is no bounding box information required\.

## Find object locations<a name="ud-object-localization"></a>

The model predicts the location of an object on an image\. The prediction includes bounding box information for the object location and a label that identifies the object within the bounding box\. For example, the following image shows bounding boxes around various parts of a circuit board, such as a *comparator* or *pot resistor*\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/localization-circuit-board.png)

To train the model, images in your training and test datasets need bounding boxes surrounding the objects that you want your model to find\. Each bounding box needs a label that identifies the type of the object\. 

The [Object localization](gs-introduction.md#gs-object-localization-example) example project shows how Amazon Rekognition Custom Labels uses labeled bounding boxes to train a model that finds object locations\.

 If you train a model that finds object locations, you don't need to do [Step 6: Identify objects, scenes and concepts with image\-level labels](tutorial-add-image-labels.md) as you don't need to use image\-level labels\.

## Find the location of brands<a name="ud-brand-detection-localization"></a>

Amazon Rekognition Custom Labels can train a model that finds the location of brands, such as logos, on an image\. You can provide training images in two ways\.
+  Images that are of the brand only\. Each image needs a single image\-level label that represents the brand name\. For example, the image\-level label for the following image of a logo could be *Lambda*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/lambda-logo.jpg)
+ Images that contain the brand in natural locations, such as a football game or an architectual diagram\. Each training image needs bounding boxes that surround each instance of the brand\. For example, the following image shows an architectural diagram with labeled bounding boxes surrounding the AWS Lambda and Amazon Pinpoint logos\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/brand-detection-lambda.png)

We recommend that you don't mix image\-level labels and bounding boxes in your training images\. 

The test images must have bounding boxes around instances of the logo\. You can split the training dataset to create the test dataset, only if the training images include labeled bounding boxes\. If the training images only have image\-level labels, you need to create a test dataset set that includes images with labeled bounding boxes\. If you train a model to find brand locations, do steps [Step 7: Locate objects with bounding boxes](tutorial-draw-bounding-boxes.md) and [Step 6: Identify objects, scenes and concepts with image\-level labels](tutorial-add-image-labels.md) according to how you label your images\. 

The [Brand detection](gs-introduction.md#gs-brand-detection-example) example project shows how Amazon Rekognition Custom Labels uses labeled bounding boxes to train a model that finds object locations\.