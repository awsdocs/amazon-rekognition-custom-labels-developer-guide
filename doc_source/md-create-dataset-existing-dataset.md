# Existing dataset<a name="md-create-dataset-existing-dataset"></a>

If you've previously created a dataset, you can copy its contents to a new dataset\.

**To create a dataset using an existing Amazon Rekognition Custom Labels dataset \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose **Projects**\.

1. In the **Projects** page, choose the project to which you want to add a dataset\. The details page for your project is displayed\.

1. Choose **Create dataset**\. The **Create dataset** page is shown\.

1. In **Starting configuration**, choose either **Start with a single dataset** or **Start with a training dataset**\. To create a higher quality model, we recommend starting with separate training and test datasets\.

------
#### [ Single dataset ]

   1. In the **Training dataset details** section, choose **Copy an existing Amazon Rekognition Custom Labels dataset**\.

   1. In the **Training dataset details** section, in the **Dataset** edit box, type or select the name of the dataset that you want to copy\. 

   1. Choose **Create Dataset**\. The datasets page for your project opens\.

------
#### [ Separate training and test datasets ]

   1. In the **Training dataset details** section, choose **Copy an existing Amazon Rekognition Custom Labels dataset**\.

   1. In the **Training dataset details** section, in the **Dataset** edit box, type or select the name of the dataset that you want to copy\. 

   1. In the **Test dataset details** section, choose **Copy an existing Amazon Rekognition Custom Labels dataset**\.

   1. In the **Test dataset details** section, in the **Dataset** edit box, type or select the name of the dataset that you want to copy\. 
**Note**  
Your training and test datasets can have different image sources\.

   1. Choose **Create Datasets**\. The datasets page for your project opens\.

------

1. If you need to add or change labels, do [Labeling images](md-labeling-images.md)\.

1. Follow the steps in [Training a model \(Console\)](training-model.md#tm-console) to train your model\.