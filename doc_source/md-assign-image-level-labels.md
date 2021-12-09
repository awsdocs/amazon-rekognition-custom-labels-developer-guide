# Assigning image\-level labels to an image<a name="md-assign-image-level-labels"></a>

You use image\-level labels to train models that classify images into categories\. An image\-level label indicates that an image contains an object, scene or concept\. For example, the following image shows a river\. If your model classifies images as containing rivers, you would add a *river* image\-level label\. For more information, see [Purposing datasets](md-dataset-purpose.md)\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/pateros.jpg)

A dataset that contains image\-level labels, needs at least two labels defined\. Each image needs at least one assigned label that identifies the object, scene, or concept in the image\.

**To assign image\-level labels to an image \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project that you want to use\. The details page for your project is displayed\.

1. On the project details page, choose **Label images**

1. If you want to add labels to your training dataset, choose the **Training** tab\. Otherwise choose the **Test** tab to add labels to the test dataset\. 

1. Choose **Start labeling** to enter labeling mode\.

1. In the image gallery, select one or more images that you want to add labels to\. You can only select images on a single page at a time\. To select a contiguous range of images on a page:

   1. Select the first image in the range\.

   1. Press and hold the shift key\.

   1. Select the last image range\. The images between the first and second image are also selected\. 

   1. Release the shift key\.

1. Choose **Assign Labels**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/select-image.png)

1. In **Assign a label to selected images** dialog box, select a label that you want to assign to the image or images\.

1. Choose **Assign image\-level labels** to assign label to the image\.

1. Repeat labeling until every image is annotated with the required labels\.

1. Choose **Save changes** to save your changes\.

1. Choose **Exit** to exit labeling mode\.

## Assign image\-level labels \(SDK\)<a name="md-assign-image-level-labels-sdk"></a>

You can use the `UpdateDatasetEntries` API to add or update the image\-level labels that are assigned to an image\. `UpdateDatasetEntries` takes one or more JSON lines\. Each JSON Line represents a single image\. For an image with an image\-level label, the JSON Line looks similar to the following\. 

```
{"source-ref":"s3://custom-labels-console-us-east-1-nnnnnnnnnn/gt-job/manifest/IMG_1133.png","TestCLConsoleBucket":0,"TestCLConsoleBucket-metadata":{"confidence":0.95,"job-name":"labeling-job/testclconsolebucket","class-name":"Echo Dot","human-annotated":"yes","creation-date":"2020-04-15T20:17:23.433061","type":"groundtruth/image-classification"}}
```

The `source-ref` field indicates the location of the image\. The JSON line also includes the image\-level labels assigned to the image\. For more information, see [Image\-Level labels in manifest files](md-create-manifest-file-classification.md)\.

**To assign image\-level labels to an image**

1. Get the get JSON Line for the existing image by using the `ListDatasetEntries`\. For the `source-ref` field, specify the location of the image that you want to assign the label to\. For more information, see [Listing dataset entries \(SDK\)](md-listing-dataset-entries-sdk.md)\. 

1. Update the JSON Line returned in the previous step using the information at [Image\-Level labels in manifest files](md-create-manifest-file-classification.md)\.

1. Call `UpdateDatasetEntries` to update the image\. For more information, see [Adding more images to a dataset](md-add-images.md)\.