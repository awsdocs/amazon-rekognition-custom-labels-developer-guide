# Step 5: Add labels<a name="tutorial-create-labels"></a>

After uploading your images into a dataset, you use the dataset gallery to add new labels to the dataset\. A label identifies an object, scene, concept, or bounding box around an object in an image\. For example, if your dataset contains images of dogs, you might add labels for breeds of dogs\. Your dataset needs at least two labels if you are using it to create a model that identifies objects, scenes, and concepts\. You only need one label if your dataset is used to create a model that finds the location of objects on an image\. For more information, see [Purposing datasets](cd-create-dataset.md#cd-dataset-purpose)\. 

**Note**  
Amazon Rekognition Custom Labels doesn't use Exif orientation metadata in images to rotate images for display\. Depending on the orientation of the camera at the time the image was photographed, images in the dataset gallery might appear with the wrong orientation\. This doesn't affect the training of your model\.

In this step, you create the labels that you assign to your images as a whole \(image\-level\) or assign to bounding boxes around objects in your images\.

**To create a label**

1. Decide on the names of the labels that you want to use\.

1. If you aren't already in labeling mode, choose **Start labeling** in the dataset gallery, to enter labeling mode\. 

1. In the **Filter by labels** section of the dataset gallery, choose **Add** to open the **Edit labels** dialog box\.

1. Enter a new label name\.

1. Choose **Add label**\.

1. Repeat steps 4 and 5 until you have created all the labels you need\. 

1. Create **Save** to save the labels that you added\.