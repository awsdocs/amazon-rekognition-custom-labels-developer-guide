# Labeling the dataset<a name="rv-editing-labels"></a>

You can use the Amazon Rekognition Custom Labels console to add, change, or remove labels from a dataset\. To add a label to a dataset, you can add a new label that you create or import labels from an existing dataset in Rekognition\.

**Topics**
+ [Add new labels](#add-new-labels)
+ [Import existing label list](#import-existing-labels)
+ [Change and remove labels](#edit-labels-after-adding)

## Add new labels<a name="add-new-labels"></a>

You can specify new labels that you want to add to your dataset\. To add your first label, you need to use the editing window\. After you have added at least one label, you can add more labels using the editing window or [use the quick add button](#quick-add-labels) to add labels without the editing window\. 

### Add labels using the editing window<a name="add-with-modal"></a>

**To add a new label \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose the project that contains the dataset you want to use\.

1. In the **Datasets** section, choose the dataset that you want to use\.

1. In the dataset gallery page, choose **Start labeling** to enter labeling mode\.

1. In the **Filter by labels** section of the dataset gallery, choose **Add** to open the **Add or import labels** dialog box\.

1. Choose **Add labels**\.

1. Enter a new label name\.

1. Choose **Add label**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/create-label.png)

1. Repeat steps 9 and 10 until you have created all the labels you need\.

1. Choose **Save** to save the labels that you added\.

### Add labels using the quick add button<a name="quick-add-labels"></a>

After you have created at least one label in the dataset, you can create more labels without entering the **Edit labels** window\.

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose the project that contains the dataset you want to use\.

1. In the **Datasets** section, choose the dataset that you want to use\.

1. In the dataset gallery page, choose **Start labeling** to enter labeling mode\.

1. Choose **Create new label**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/label-quick-add.png)

1. Enter a new label name\. After you use the return key, the label will appear in your list of labels\.

1. Repeat steps 7\-8 until you have created all the labels you need\.

1. Choose **Save changes** to save the labels that you added\.

## Import existing label list<a name="import-existing-labels"></a>

If you created labels in one dataset and you want to use the same labels for images in another dataset, you can import the label list from the first dataset into the second\.

**Note**  
You can only import a label list into a dataset that doesn't have any labels\.

**To import an existing label list \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose the project that contains the dataset you want to use\.

1. In the **Datasets** section, choose the dataset that you want to use\.

1. In the dataset gallery page, choose **Start labeling** to enter labeling mode\.

1. In the **Filter by labels** section of the dataset gallery, choose **Add**\.

1. In **Add or import labels**, choose **Import label list**\.

1. Choose a dataset\. The labels in that dataset appear in the window\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/import-labels.png)
**Note**  
If the dataset you choose doesn't have any labels, an error message appears\. You can choose another dataset to import from, or switch to **Add labels** to create new labels\.

1. Choose **Save** to save the label list into your dataset\.

## Change and remove labels<a name="edit-labels-after-adding"></a>

You can rename or remove labels after adding them to a dataset\.

**To rename or remove an existing label \(console\)**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. Choose **Use Custom Labels**\.

1. Choose **Get started**\. 

1. In the left navigation pane, choose the project that contains the dataset you want to use\.

1. In the **Datasets** section, choose the dataset that you want to use\.

1. In the dataset gallery page, choose **Start labeling** to enter labeling mode\.

1. In the **Filter by labels** section of the dataset gallery, choose **Edit**\. The **Edit labels** dialog box opens\.

1. In **Edit labels**, choose the icon to rewrite or delete the label\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/edit-labels.png)

1. \(Optional\) You can rename the label by selecting the existing label name and then entering the label name you want to use\.

1. Choose **Save** to save the labels into your dataset\.