# Step 5: Identify Objects, Scenes and Concepts with Image\-Level Labels<a name="gs-add-image-labels"></a>

To train your model to detect objects, scenes, and concepts in your images, Amazon Rekognition Custom Labels requires the images in your dataset to be labeled with image\-level labels\. 

**Note**  
If you're training a model to detect the location of objects on an image, you don't need to do this step\.

An image\-level label applies to an image as a whole\. For example, to train a model to detect scenic views, the labels for the following image might be *river*, *countryside* or *sky*\. An image needs at least one label\. Your dataset needs at least two labels\. In this step, you add image\-level labels to an image\. 

 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/pateros.png)

**To assign image\-level labels to images**

1. In the dataset gallery, select one or more images that you want to add labels to\. You can only select images on a single page at a time\. To select the images between a first and second image on a page:

   1. Select the first image\.

   1. Press and hold the shift key\.

   1. Select the second image\. The images between the first and second image are also selected\. 

   1. Release the shift key\.

1. Choose **Assign labels**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/select-image.png)

1. In the **Assign a label to selected images** dialog box, choose a label that you want to assign to the image or images\.

1. Choose **Assign** to assign the label to the image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/assign-river.png)

1. Repeat labeling until every image is assigned the required labels\.

1. Choose **Save changes** to save your changes\.