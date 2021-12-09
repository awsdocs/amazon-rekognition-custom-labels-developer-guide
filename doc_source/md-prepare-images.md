# Preparing images<a name="md-prepare-images"></a>

 The images in your training and test dataset contain the objects, scenes, or concepts that you want your model to find\. 

The content of images should be in a variety of backgrounds and lighting that represent the images that you want the trained model to identify\.

This section provides information about the images in your training and test dataset\.

## Image format<a name="pi-image-format"></a>

You can train Amazon Rekognition Custom Labels models with images that are in PNG and in JPEG format\. Similarly, to detect custom labels using `DetectCustomLabels`, you need images that are in PNG and JPEG format\.

## Input image recommendations<a name="md-image-recommendations"></a>

### <a name="pi-images-recommmendations"></a>

Amazon Rekognition Custom Labels requires images to train and test your model\. To prepare your images, consider following:
+ Choose a specific domain for the model you want to create\. For example, you could choose a model for scenic views and another model for objects such as machine parts\. Amazon Rekognition Custom Labels works best if your images are in the chosen domain\.
+ Use at least 10 images to train your model\.
+ Images must be in PNG or JPEG format\.
+ Use images that show the object in a variety of lightings, backgrounds, and resolutions\.
+ Training and testing images should be similar to the images that you want to use the model with\. 
+ Decide what labels to assign to the images\.
+ Ensure that images are sufficiently large in terms of resolution\. For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\.
+ Ensure that occlusions don't obscure objects that you want to detect\.
+ Use images that have sufficient contrast with the background\. 
+ Use images that are bright and sharp\. Avoid using images that may be blurry due to subject and camera motion as much as possible\.
+ Use an image where the object occupies a large proportion of the image\.
+ Images in your test dataset shouldn't be images that are in the training dataset\. They should include the objects, scenes, and concepts that the model is trained to analyze\.

## Image set size<a name="md-set"></a>

Amazon Rekognition Custom Labels uses a set of images to train a model\. At a minimum, you should use at least 10 images for training\. Amazon Rekognition Custom Labels stores training and testing images in datasets\. For more information, see [Creating training and test datasets \(Console\)](md-create-dataset.md)\.