# Step 4: Create a training dataset<a name="tutorial-step-create-dataset"></a>

In this step, you create a training [Dataset](ud-terminology.md#ud-dataset) that holds your images and the label data needed to train your model\. You add images to the dataset using images that you upload from your computer\. You can also add images and label data to a dataset in other ways\. For more information, see [Creating an Amazon Rekognition Custom Labels dataset](cd-create-dataset.md)\. To train your model, you need a test dataset\. In this tutorial, you split the training dataset to create the test dataset\. Alternatively, you can create a separate test dataset\. For more information, see see [Training an Amazon Rekognition Custom Labels model](tm-train-model.md)\.

For more information about the workflow for creating and managing a dataset, see [Managing an Amazon Rekognition Custom Labels dataset](cd-managing-datasets.md)\.

To ensure that Amazon Rekognition Custom Labels chooses the best algorithm to train your model, we recommend that you use domain\-specific datasets\. For example, create a dataset specifically for detecting scenic views, and use another dataset for a model that detects the location of a logo on an image\. For more information, see [Purposing datasets](cd-create-dataset.md#cd-dataset-purpose)\. 

The images you upload won't have labels associated with them\. You add them in subsequent steps\.

**Note**  
If you have many images to upload, consider using an Amazon S3 bucket\. For more information, see [Amazon S3 bucket](cd-s3.md)\.<a name="tutorial-create-dataset-procedure"></a>

**To create a training dataset \(console\)**

1. In the **Datasets** section of the project page, choose **Create dataset**\. 

1. On the **Create Dataset** details page, enter a name for your dataset\.

1. Choose **Upload images from your computer\. **

1. Choose **Submit**\. 

1. A **Tool Guide** is shown\. Choose **Next** to read each page\. After you have finished reading, the dataset gallery is shown\. The gallery is placed in labeling mode so that you can add your images\. 

1. Choose **\+ Add Images**\. The **Add images** dialog box is shown\.

1. Drag and drop your image onto the dialog box, or choose **Choose files** to select the images from your computer\. 

    You can load up to 30 images at a time\.

1. Choose **Add images**\. The images are uploaded and added to the dataset\. You should see the dataset gallery page with your images\.

1. If you need to add more images, do steps 6\-8 until all of your images are added\. 