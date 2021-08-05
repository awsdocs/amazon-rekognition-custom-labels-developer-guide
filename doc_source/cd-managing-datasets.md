# Managing an Amazon Rekognition Custom Labels dataset<a name="cd-managing-datasets"></a>

In an Amazon Rekognition Custom Labels project, datasets contain the images, assigned labels, and bounding boxes that you use to train and test a model\. To train a model, you need a training dataset and a test dataset\. The test dataset can be created by splitting the training dataset\. You create and manage datasets by using the Amazon Rekognition Custom Labels console\. You can't create a dataset with the Amazon Rekognition Custom Labels API\.

The following sections describe the workflow for creating and managing a dataset\.

## Prepare your images<a name="cd-prepare-images"></a>

 You need images to train and test the model\. The images represent the objects, scenes, and concepts that you want the model to detect\. For recommendations about images, see [Preparing images](pi-prepare-images.md)\. 

## Create a dataset<a name="cd-datasets"></a>

You create a dataset by specifying where to import the images and labels from\. You can import from the following locations:
+ An S3 bucket 
+ Your computer
+ An Amazon SageMaker Ground Truth format manifest file
+ An existing dataset 

For more information, see [Creating an Amazon Rekognition Custom Labels dataset](cd-create-dataset.md)\. 

To train a model, your training and test datasets needs images with labels assigned to them\. A label identifies the content of an image\. For example, if an image contains an image of a bridge, you assign a 'bridge' label to the image\. For more information, see [Purposing datasets](cd-create-dataset.md#cd-dataset-purpose)\.

## Review your images<a name="rv-images"></a>

The dataset gallery shows the images in a dataset\. If the images don't have labels, or if you want to create new label categories, you can create them in the dataset gallery\. For more information, see [Labeling the dataset](rv-editing-labels.md)\.

If you imported your images with accompanying labels, the labels are shown in the left pane of the dataset gallery\. You assign labels to images\. For more information, see [Assigning image\-level labels to an image](rv-assign-labels.md)\. You also assign labels to areas of an image that represent an object\. To assign labels to an object, you use the Amazon Rekognition Custom Labels drawing tool to draw a bounding box around the object\. For more information, see [Drawing bounding boxes](rv-bounding-box.md)\. 

Amazon Rekognition Custom Labels doesn't use Exif orientation metadata in images to rotate images for display\. Depending on the orientation of the camera at the time the image was photographed, images in the dataset gallery might appear with the wrong orientation\. This doesn't affect the training of your model\.

After you've labeled your images, you can train your model\. For more information, see [Training an Amazon Rekognition Custom Labels model](tm-train-model.md)\.