# \(Optional\) Step 7: Associate prior datasets with new projects<a name="su-associate-prior-dataset"></a>

Amazon Rekognition Custom Labels now manages datasets with projects\. Earlier \(prior\) datasets that you created are read\-only and must be associated with a project before you can use them\. When you open the details page for a project with the console, we automatically associate the datasets that trained the latest version of the project's model with the project\. Automatic association of a dataset with a project doesn't happen if you are using the AWS SDK\.

Unassociated prior datasets have never been used to train a model or, were used to train a previous version of a model\. The Prior datasets page shows all of your associated and unassociated datasets\.

To use an unassociated prior dataset, you create a new project on the Prior datasets page\. The dataset becomes the training dataset for the new project\. You can also create a project for an already associated dataset as prior datasets can have multiple associations\. 

**To associate a prior dataset to a new project**

1. Open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. In the left pane, choose Use **Custom Labels**\. The Amazon Rekognition Custom Labels landing page is shown\. 

1. In the left navigation pane, choose **Prior datasets**\.

1. In the datasets view, choose the prior dataset that you want to associate with a project\.

1. Choose **Create project with dataset**\.

1. On the **Create project** page, enter a name for your new project in **Project name**\.

1. Choose **Create project** to create the project\. The project might take a while to create\.

1. Use the project\. For more information, see [Understanding Amazon Rekognition Custom Labels](understanding-custom-labels.md)\. 

## Using a prior dataset as a test dataset<a name="su-prior-dataset-as-test-dataset"></a>

You can use a prior dataset as the test dataset for an existing project by first associating the prior dataset with a new project\. You then copy the training dataset of the new project to the test dataset of the existing project\.

**To use a prior dataset as a test dataset**

1. Follow the instructions at [\(Optional\) Step 7: Associate prior datasets with new projects](#su-associate-prior-dataset) to associate the prior dataset with a new project\. 

1. Create the test dataset in the existing project by using copying the training dataset from the new project\. For more information, see [Existing dataset](md-create-dataset-existing-dataset.md)\.

1. Follow the instructions at [Deleting an Amazon Rekognition Custom Labels project \(Console\)](mp-delete-project.md#mp-delete-project-console) to delete the new project\. 

Alternatively, you can create the test dataset by using the manifest file for prior dataset\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.