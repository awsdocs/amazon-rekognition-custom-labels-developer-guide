# Step 1: Choose an example project<a name="gs-step-choose-example-project"></a>

In this step you use choose an example project\. Amazon Rekognition Custom Labels then creates a project and a dataset for you\. A project manages the files used to train your model\. For more information, see [Managing an Amazon Rekognition Custom Labels Project](cp-manage-project.md)\. Datasets contain the images, assigned labels, and bounding boxes that you use to train and test a model\. For more information, see [Managing an Amazon Rekognition Custom Labels dataset](cd-managing-datasets.md)\. 

For information about the example projects, see [Example projects](gs-introduction.md#gs-example-projects)\.

**Choose an example project**

1. Sign in to the AWS Management Console and open the Amazon Rekognition console at [https://console\.aws\.amazon\.com/rekognition/](https://console.aws.amazon.com/rekognition/)\.

1. In the left pane, choose **Use Custom Labels**\. The Amazon Rekognition Custom Labels landing page is shown\. If you don't see **Use Custom Labels**, check that the [ AWS Region](https://docs.aws.amazon.com/general/latest/gr/rekognition_region.html) you are using supports Amazon Rekognition Custom Labels\.

1. Choose **Get started**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/example-projects.png)

1. In **Explore example projects**, choose **Try example projects**\.

1. Decide which project you want to use and choose **Create project "*project name*"** within the example section\. Amazon Rekognition Custom Labels then creates the example project for you\.
**Note**  
If this is the first time that you've opened the console in the current AWS Region, the **First Time Set Up** dialog box is shown\. Do the following:  
Note the name of the Amazon S3 bucket that's shown\.
Choose **Continue** to let Amazon Rekognition Custom Labels create an Amazon S3 bucket \(console bucket\) on your behalf\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/get-started.jpg)

1. After your project is ready, choose **Go to dataset**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/get-started-goto-dataset-dialog.jpg)